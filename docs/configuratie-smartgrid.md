---
title: Energiebeheer
---

# Energiebeheer

Op het tabblad **Energiebeheer** stel je in hoe OpenAmber reageert op signalen van buitenaf over energie: **SG Ready** en **EEBUS**. Daarmee kan de warmtepomp extra warmte opslaan als stroom goedkoop of zelf opgewekt is, en pauzeren als het net of het tarief daarom vraagt.

Het tabblad heeft twee categorieën: **SG-Ready** en **EEBUS**.

## SG Ready

OpenAmber kent vier SG Ready-standen. Een stand kan op twee manieren actief worden:

1. via de twee **SG Ready-contacten** op de binnenunit (A: register 1211, B: register 1220), bijvoorbeeld vanuit een omvormer of energiemanager;
2. via de keuze **SG-Ready modus** (`select.openamber_sg_ready_modus`), bijvoorbeeld vanuit een Home Assistant automatisering of EEBUS.

| Stand | Contact A | Contact B | Wat doet OpenAmber? |
| :-- | :-- | :-- | :-- |
| **Normaal** | uit | uit | Gewone regeling. |
| **Blokkeren** | aan | uit | Geen warmtevraag, geen koudevraag en geen tapwater. |
| **Boost** | uit | aan | Verwarmingssetpoint en tapwatersetpoint omhoog met de boostwaarden hieronder. |
| **Max boost** | aan | aan | Als Boost, en daarnaast wordt het backupelement bijgeschakeld bij verwarmen en tapwater. |

De contacten en de keuze werken naast elkaar: een stand is actief als het contact **of** de keuze erom vraagt.

::: warning SG-Ready modus na herstart
De keuze *SG-Ready modus* wordt niet bewaard en staat na een herstart weer op **Normaal**. Laat je automatisering de stand dus periodiek opnieuw zetten.
:::

### Instellingen SG-Ready

![SmartGrid](/images/instellingen-smartgrid.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Verwarmingsboost (°C)** | 0 t/m 10 °C (stap 1) | 5 °C | `number.openamber_sg_ready_verwarmingsboost_setpoint` | Verhoging van de cv-aanvoertemperatuur bovenop het actieve stooklijn- of externe setpoint tijdens Boost en Max boost. Zo slaat de vloer extra warmte op. Beperk de boost om oververhitting in huis te voorkomen. |
| **Tapwater boost (°C)** | 0 t/m 25 °C (stap 1) | 5 °C | `number.openamber_sg_ready_tapwaterboost_setpoint` | Verhoging van het tapwatersetpoint tijdens Boost en Max boost. Zo laadt de boiler extra door met goedkope stroom. |

---

## EEBUS

Met EEBUS koppelt OpenAmber rechtstreeks met een energiemanagementsysteem (EMS), zoals [evcc](https://evcc.io). Het EMS kan de warmtepomp dan laten boosten of afschalen, en leest het vermogen en de tapwatertemperatuur uit.

Wat het EMS kan doen:

- **Boost vragen** (zelfverbruik optimaliseren): OpenAmber zet *SG-Ready modus* op **Boost**. Stopt het EMS de boost, dan gaat de stand terug naar **Normaal**.
- **Vermogen beperken** (netbegrenzing): OpenAmber zet *SG-Ready modus* op **Blokkeren**. Wordt de beperking opgeheven, dan gaat de stand terug naar **Normaal**.
- **Meetwaarden uitlezen**: actueel vermogen, energie, spanning, stroom en tapwatertemperatuur.

Valt de verbinding met het EMS weg, dan heft OpenAmber een beperking na 2 uur zelf op.

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **EEBUS inschakelen** | Aan / Uit | Uit | `switch.openamber_eebus_ingeschakeld` | Zet de EEBUS-communicatie aan. OpenAmber is daarna in het netwerk te vinden voor het EMS. |
| **Verbinding met EMS** | status, knop *Verbreek verbinding* | — | *EEBUS Verbonden*, *EEBUS EMS* | Laat zien of en met welk EMS OpenAmber verbonden is. Met de knop verbreek je de koppeling. |
| **SKI** | tekst | — | *EEBUS SKI* | De *Subject Key Identifier* van OpenAmber. Die heb je nodig om OpenAmber in je EMS te koppelen. |
| **EEBUS status** | tekst | — | *EEBUS Status*, *EEBUS Laatste actie* | Bedrijfstoestand van de koppeling en de laatste opdracht van het EMS. |
| **Vermogenslimiet** | W | — | *EEBUS Vermogenslimiet* | De netbegrenzing die het EMS op dit moment vraagt. |
