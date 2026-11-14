---
title: Integratie met Home Assistant
---

# Integratie met Home Assistant

Deze pagina focust op geavanceerde aansturing van OpenAmber vanuit Home Assistant.

Doelen:

1. Betrouwbare koppeling en veilige basis.
2. Dynamische regeling voor verwarmen en koelen.
3. Slimme energie-aansturing op dynamische tarieven zonder fysieke SG-relais.

## Basisprincipes

Belangrijke OpenAmber-entiteitstypen:

1. Schakelaars: `switch.*`
2. Numerieke instellingen: `number.*`
3. Keuzes: `select.*`
4. Tijdwaarden (schema): `time.*`

### Essentiele randvoorwaarden

Controleer voor geavanceerde automatiseringen:

1. `select.openamber_verwarmingsmodus` staat op de juiste modus.
2. `select.openamber_koelmodus` staat op de juiste modus.
3. Thermostaatbron (`select.openamber_thermostaat_keuze`) past bij je strategie.

::: tip Entity-id's
De entity-id's op deze pagina gaan uit van de standaard apparaatnaam `OpenAmber`. Zie [Configuratie](./configuratie.html#home-assistant-entiteiten) als jouw namen afwijken.
:::

::: warning Waarden die na een herstart terugspringen
`number.openamber_handmatig_setpoint_cv_aanvoertemperatuur` (extern setpoint) springt na een herstart terug naar 30 °C, en `select.openamber_sg_ready_modus` naar *Normaal*. Geef automatiseringen daarom altijd ook een tijdtrigger, zoals in de voorbeelden hieronder, zodat de waarde na een herstart vanzelf weer goed komt.
:::

Voorbeeld:

1. Wil je `number.openamber_handmatig_setpoint_cv_aanvoertemperatuur` dynamisch sturen voor verwarmen: zet `select.openamber_verwarmingsmodus` op `Extern setpoint`.
2. Wil je extern koelsetpoint sturen via `number.openamber_handmatig_setpoint_cv_aanvoertemperatuur`: zet `select.openamber_koelmodus` op `Extern setpoint`.

## Voorbeeld 1: Dynamische stooklijn via Home Assistant

Doel:

1. Op basis van buitentemperatuur een dynamisch aanvoersetpoint bepalen.
2. Dat setpoint wegschrijven naar `number.openamber_handmatig_setpoint_cv_aanvoertemperatuur`.

Vereiste OpenAmber-instelling:

1. `select.openamber_verwarmingsmodus` = `Extern setpoint`.

### Input helper

Maak een helper voor minimum en maximum aanvoer:

```yaml
input_number:
  oa_min_aanvoer:
    name: OA minimum aanvoer
    min: 20
    max: 45
    step: 1
    unit_of_measurement: "degC"
    initial: 25
  oa_max_aanvoer:
    name: OA maximum aanvoer
    min: 20
    max: 55
    step: 1
    unit_of_measurement: "degC"
    initial: 40
```

### Template sensor voor dynamisch setpoint

Pas de buiten-temperatuursensor naar jouw situatie aan.

```yaml
template:
  - sensor:
      - name: oa_dynamisch_verwarmen_setpoint
        unit_of_measurement: "degC"
        state: >-
          {% set ta = states('sensor.buitentemperatuur') | float(10) %}
          {% set t_min = states('input_number.oa_min_aanvoer') | float(25) %}
          {% set t_max = states('input_number.oa_max_aanvoer') | float(40) %}
          {% set raw = t_max - ((ta + 10) * (t_max - t_min) / 25) %}
          {% set clipped = [t_max, [raw, t_min] | max] | min %}
          {{ clipped | round(0) }}
```

### Automatisering naar OpenAmber

```yaml
automation:
  - id: oa_dynamische_stooklijn_naar_manual_setpoint
    alias: OpenAmber dynamische stooklijn naar manual_setpoint
    mode: restart
    trigger:
      - platform: state
        entity_id:
          - sensor.oa_dynamisch_verwarmen_setpoint
      - platform: time_pattern
        minutes: "/10"
    condition:
      - condition: state
        entity_id: select.openamber_verwarmingsmodus
        state: Extern setpoint
    action:
      - action: number.set_value
        target:
          entity_id: number.openamber_handmatig_setpoint_cv_aanvoertemperatuur
        data:
          value: "{{ states('sensor.oa_dynamisch_verwarmen_setpoint') | float(30) }}"
```

## Voorbeeld 2: Koelen op basis van dauwpunt

Doel:

1. Condensatierisico beperken.
2. Koelsetpoint adaptief maken op basis van dauwpunt + veiligheidsmarge.

Vereiste OpenAmber-instelling:

1. `select.openamber_koelmodus` = `Extern setpoint`.

### Benodigde sensoren

1. Ruimtetemperatuur.
2. Relatieve luchtvochtigheid.

### Dauwpunt-template

```yaml
template:
  - sensor:
      - name: oa_dauwpunt
        unit_of_measurement: "degC"
        state: >-
          {% set t = states('sensor.kamer_temperatuur') | float(23) %}
          {% set rh = states('sensor.kamer_luchtvochtigheid') | float(55) %}
          {% set a = 17.27 %}
          {% set b = 237.7 %}
          {% set alpha = ((a * t) / (b + t)) + (rh / 100) | log %}
          {{ ((b * alpha) / (a - alpha)) | round(1) }}
      - name: oa_koel_setpoint_dauwpuntveilig
        unit_of_measurement: "degC"
        state: >-
          {% set dp = states('sensor.oa_dauwpunt') | float(15) %}
          {% set marge = 2.0 %}
          {% set target = dp + marge %}
          {% set min_allowed = 16 %}
          {% set max_allowed = 24 %}
          {{ [max_allowed, [target, min_allowed] | max] | min | round(1) }}
```

### Automatisering naar extern koelsetpoint

```yaml
automation:
  - id: oa_dauwpunt_koeling_naar_manual_setpoint
    alias: OpenAmber dauwpuntgestuurd koelen
    mode: restart
    trigger:
      - platform: state
        entity_id:
          - sensor.oa_koel_setpoint_dauwpuntveilig
      - platform: time_pattern
        minutes: "/10"
    condition:
      - condition: state
        entity_id: select.openamber_koelmodus
        state: Extern setpoint
    action:
      - action: number.set_value
        target:
          entity_id: number.openamber_handmatig_setpoint_cv_aanvoertemperatuur
        data:
          value: "{{ states('sensor.oa_koel_setpoint_dauwpuntveilig') | float(19) }}"
```

## Voorbeeld 3: Dynamische tarieven met SG-modus (zonder relais)

Doel:

1. Geen fysieke SG-ready ingangen gebruiken.
2. Toch sturen op goedkope en dure uren via `select.openamber_sg_ready_modus`.

SG-modi in OpenAmber:

1. Normaal
2. Blokkeren
3. Boost
4. Max boost

### Input helper voor prijsdrempels

```yaml
input_number:
  oa_prijs_goedkoop:
    name: OA prijs goedkoop
    min: -1
    max: 1
    step: 0.01
    unit_of_measurement: "EUR/kWh"
    initial: 0.10
  oa_prijs_duur:
    name: OA prijs duur
    min: -1
    max: 2
    step: 0.01
    unit_of_measurement: "EUR/kWh"
    initial: 0.35
```

### Automatisering op basis van dynamische prijs

Vervang de prijs-entiteit door die van jouw energieleverancier/integratie.

```yaml
automation:
  - id: oa_sg_mode_op_dynamische_prijs
    alias: OpenAmber SG modus op dynamische prijs
    mode: single
    trigger:
      - platform: state
        entity_id: sensor.energieprijs_actueel
      - platform: time_pattern
        minutes: "/15"
    variables:
      prijs: "{{ states('sensor.energieprijs_actueel') | float(0.25) }}"
      goedkoop: "{{ states('input_number.oa_prijs_goedkoop') | float(0.10) }}"
      duur: "{{ states('input_number.oa_prijs_duur') | float(0.35) }}"
    action:
      - choose:
          - conditions:
              - condition: template
                value_template: "{{ prijs <= goedkoop }}"
            sequence:
              - action: select.select_option
                target:
                  entity_id: select.openamber_sg_ready_modus
                data:
                  option: Boost
          - conditions:
              - condition: template
                value_template: "{{ prijs >= duur }}"
            sequence:
              - action: select.select_option
                target:
                  entity_id: select.openamber_sg_ready_modus
                data:
                  option: Blokkeren
        default:
          - action: select.select_option
            target:
              entity_id: select.openamber_sg_ready_modus
            data:
              option: Normaal
```

## Voorbeeld 4: Dynamische tarieven met dynamisch setpoint (zonder SG)

Alternatief voor SG-modus:

1. Bij goedkope uren iets hoger verwarmingssetpoint.
2. Bij dure uren terug naar nominale waarde.

```yaml
automation:
  - id: oa_setpoint_shift_op_prijs
    alias: OpenAmber setpoint shift op prijs
    mode: restart
    trigger:
      - platform: state
        entity_id: sensor.energieprijs_actueel
      - platform: time_pattern
        minutes: "/15"
    condition:
      - condition: state
        entity_id: select.openamber_verwarmingsmodus
        state: Extern setpoint
    variables:
      prijs: "{{ states('sensor.energieprijs_actueel') | float(0.25) }}"
      basis: 30
      shift: >-
        {% if prijs <= 0.10 %}
          2
        {% elif prijs >= 0.35 %}
          -2
        {% else %}
          0
        {% endif %}
    action:
      - action: number.set_value
        target:
          entity_id: number.openamber_handmatig_setpoint_cv_aanvoertemperatuur
        data:
          value: "{{ (basis + shift) | float }}"
```

## Voorbeeld 5: Warmtevraag vanuit Home Assistant

Doel:

1. Geen bedrade kamerthermostaat gebruiken.
2. Een thermostaat of temperatuursensor in Home Assistant laten bepalen wanneer er warmtevraag is.

Vereiste OpenAmber-instelling:

1. `select.openamber_thermostaat_keuze` = `Extern`.

OpenAmber heeft dan warmtevraag zodra de schakelaar `switch.openamber_externe_warmtevraag` aan staat, of het bedrade contact gesloten is. Zie [Thermostaat](./configuratie-thermostaat.html#hoe-de-warmtevraag-ontstaat) voor de werking.

```yaml
automation:
  - id: oa_warmtevraag_uit_ha
    alias: OpenAmber warmtevraag vanuit Home Assistant
    mode: restart
    trigger:
      - platform: state
        entity_id: climate.woonkamer
        attribute: hvac_action
      - platform: time_pattern
        minutes: "/5"
    action:
      - choose:
          - conditions:
              - condition: state
                entity_id: climate.woonkamer
                attribute: hvac_action
                state: heating
            sequence:
              - action: switch.turn_on
                target:
                  entity_id: switch.openamber_externe_warmtevraag
        default:
          - action: switch.turn_off
            target:
              entity_id: switch.openamber_externe_warmtevraag
```

Vervang `climate.woonkamer` door je eigen thermostaat. De schakelaar staat na een herstart van OpenAmber uit; de tijdtrigger zet hem dan weer goed.

## Debuggen en veilig testen

Aanbevolen testvolgorde:

1. Eerst alleen waarden loggen in template-sensors.
2. Daarna automations activeren met lage impact (kleine stapgrootte).
3. Pas daarna agressievere boosts of blokkeringen gebruiken.

Praktische checks:

1. Controleer of de juiste modus actief is voordat je setpoints schrijft.
2. Gebruik tijdsgebaseerde trigger als vangnet bij gemiste state-events.
3. Beperk updatefrequentie om onrustig gedrag te voorkomen.
