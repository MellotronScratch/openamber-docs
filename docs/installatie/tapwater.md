---
title: Tapwater instellen
---

# Tapwater instellen

Open [Instellingen - Tapwater](./configuratie-tapwater.html).

## Basiscomfort

1. `number.openamber_tapwater_temperatuur_setpoint` instellen.
2. `number.openamber_t_herstart_tapwater_verwarmen` instellen.

## Vermogen en pompgedrag

1. `select.openamber_tapwater_basisvermogen` voor basisvermogen.
2. `select.openamber_tapwater_pomp_startmodus` voor pompstartstrategie.

## Schema instellen

1. `switch.openamber_tapwater_schema_actief` aan of uit.
2. Dagen selecteren (`switch.dhw_schedule_*_enabled_switch`).
3. Venster kiezen met `time.openamber_tapwater_starttijd` en `time.openamber_tapwater_eindtijd`.

## Legionella

1. `switch.openamber_legionellapreventie` activeren volgens beleid.
2. Interval via `number.openamber_legionellapreventie_herhaling_dagen`.
3. Doeltemperatuur via `number.openamber_legionellapreventie_doeltemperatuur`.

Volgende stap: [Koelen (optioneel)](./koelen.html).
