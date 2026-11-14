---
title: Configuratie & Instellingen
---

# Configuratie en instellingen

Deze sectie beschrijft alle instellingen uit de OpenAmber gebruikersinterface (UI) en hun koppeling met Home Assistant. OpenAmber geeft je volledige controle over het gedrag, het rendement en het comfort van je Itho Daalderop Amber warmtepomp.

::: tip Synchronisatie met Home Assistant
De meeste instellingen op het touchscreen zijn ook in Home Assistant beschikbaar als entiteit. Wijzigingen werken in beide richtingen. Instellingen van OpenAmber zelf worden bewaard in het flashgeheugen (NVS) van de ESP32; instellingen van de buitenunit (EEPROM-registers) worden via Modbus naar de buitenunit geschreven.

Een paar instellingen bestaan alleen op het scherm, bijvoorbeeld de deadband en overrun van de interne thermostaat. In de tabellen staat dan *alleen scherm*.
:::

## Systeempagina's overzicht

Klik op een categorie om direct naar de gedetailleerde instellingenpagina met toelichting, standaardwaarden en Home Assistant entiteiten te gaan:

<div class="config-grid">
  <a class="config-card" href="./configuratie-algemeen.html">
    <h3>⚙️ Algemeen</h3>
    <p>Systeemparameters, regeltemperatuurbron, mengventielen zone 1 & 2, sensorkalibratie, stille modus en flowsensor.</p>
  </a>
  <a class="config-card" href="./configuratie-verwarmen.html">
    <h3>🔥 Verwarmen</h3>
    <p>Verwarmingsmodus, stooklijn voor zone 1 en zone 2, compressorvermogen, start/stop delta's en noodbedrijf.</p>
  </a>
  <a class="config-card" href="./configuratie-koelen.html">
    <h3>❄️ Koelen</h3>
    <p>Koelmodus, intern of extern koelsetpoint, compressorvermogen, start/stop delta's en condensatiebeveiliging.</p>
  </a>
  <a class="config-card" href="./configuratie-tapwater.html">
    <h3>🚿 Tapwater</h3>
    <p>Boilertemperatuur, herstart delta, weekschema met tijdsvensters, wintermodus en legionellapreventie.</p>
  </a>
  <a class="config-card" href="./configuratie-pomp.html">
    <h3>🔄 Pomp</h3>
    <p>Circulatiepomp P0 & P1, spoelintervallen, vaste toerentallen, dynamische delta-T PID en tweetraps vorstbeveiliging.</p>
  </a>
  <a class="config-card" href="./configuratie-bijverwarmen.html">
    <h3>⚡ Bijverwarmen</h3>
    <p>Backupelement (intern elektrisch element of externe bron), ontdooiboost, graadminutendrempel en tapwater-ondersteuning.</p>
  </a>
  <a class="config-card" href="./configuratie-thermostaat.html">
    <h3>🌡️ Thermostaat</h3>
    <p>Interne thermostaat of extern signaal (bedrade thermostaat / Home Assistant), uitschakelvertraging, draadloze thermostaat, deadbands en overruns.</p>
  </a>
  <a class="config-card" href="./configuratie-smartgrid.html">
    <h3>⚡ Energiebeheer</h3>
    <p>SG Ready (boost en blokkeren) en EEBUS-koppeling met een energiemanagementsysteem.</p>
  </a>
  <a class="config-card" href="./configuratie-geavanceerd.html">
    <h3>🔧 Geavanceerd</h3>
    <p>PID-parameters, bodemplaatverwarming, defrost, compressor (hysterese, buitenunit optimalisaties) en ventilatortoerentallen.</p>
  </a>
</div>

---

## Uitleg van de tabelkolommen

Op elke configuratiepagina vind je tabellen met de volgende gegevens:

| Kolom | Betekenis |
| :-- | :-- |
| **Instelling** | De naam zoals deze op het OpenAmber touchscreen verschijnt. |
| **Opties / Bereik** | De toegestane waarden of het bereik, inclusief eenheid en stapgrootte. |
| **Standaard** | De fabrieksinstelling van OpenAmber. |
| **Home Assistant entiteit** | De entity-id in Home Assistant, of *alleen scherm* als de instelling niet in Home Assistant staat. |
| **Beschrijving** | Uitleg over de werking en advies voor het inregelen. |

## Home Assistant entiteiten

Home Assistant maakt de entity-id uit de **apparaatnaam** en de **naam van de entiteit**. Bij een standaardinstallatie heet het apparaat `OpenAmber`, dus alle entiteiten beginnen met `openamber_`:

| Naam in Home Assistant | Entity-id |
| :-- | :-- |
| Thermostaat keuze | `select.openamber_thermostaat_keuze` |
| Tapwater temperatuur setpoint | `number.openamber_tapwater_temperatuur_setpoint` |
| Stooklijn @ +15°C | `number.openamber_stooklijn_15degc` |

Heb je je apparaat een andere naam gegeven, of de entiteit in Home Assistant hernoemd, dan wijkt de entity-id af. Zoek bij twijfel op de naam via *Instellingen → Apparaten & diensten → Entiteiten*.

::: tip Uitgeschakelde entiteiten
Instellingen die rechtstreeks in de EEPROM van de buitenunit schrijven (defrost, ventilator), de onderhoudsfuncties en de draadloze thermostaat staan in Home Assistant standaard **uitgeschakeld**. Schakel ze in via de entiteitsinstellingen als je ze wilt gebruiken.
:::

Gebruikte entiteitstypen:

- **`switch.*`**: aan/uit-instellingen (bijv. `switch.openamber_tapwater_aanwezig`).
- **`number.*`**: numerieke instellingen zoals temperaturen, delta's en PID-parameters.
- **`select.*`**: keuzemenu's (bijv. `select.openamber_verwarmingsmodus`).
- **`time.*`**: tijdstippen voor schema's (bijv. `time.openamber_tapwater_starttijd`).
- **`sensor.*`** en **`binary_sensor.*`**: meetwaarden en statussen.

::: warning Pas op met uiterste waarden
Wijzig geavanceerde parameters (zoals PID-regeling, defrost-drempels en delta's) altijd in kleine stappen en observeer het systeemgedrag gedurende minimaal 24 tot 48 uur voordat je verdere aanpassingen doorvoert.
:::
