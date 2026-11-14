---
title: Verwarmen
---

# Verwarmen

De verwarmingsinstellingen bepalen hoe de warmtepomp reageert op de warmtevraag in huis. Je kunt kiezen tussen een weersafhankelijke stooklijn of een vast/extern setpoint, het maximale compressorvermogen begrenzen en de start/stop-temperatuurdrempels finetunen om pendelen te minimaliseren.

::: tip Home Assistant synchronisatie
Alle onderstaande instellingen zijn via het touchscreen en via Home Assistant aanpasbaar. Wijzigingen worden direct doorgevoerd en opgeslagen.
:::

Het tabblad heeft vijf categorieën: **Algemeen**, **Stooklijn Z1**, **Stooklijn Z2**, **Start/Stop** en **Noodbedrijf**. De PID-instellingen voor verwarmen staan onder [Geavanceerd](./configuratie-geavanceerd.html).

## Categorie: Algemeen

![Verwarmen algemeen](/images/instellingen-verwarmen-algemeen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Verwarmingsmodus** | Stooklijn / Extern setpoint | Stooklijn | `select.openamber_verwarmingsmodus` | Bepaalt hoe de doeltemperatuur voor de cv-aanvoer berekend wordt. Bij `Stooklijn` regelt OpenAmber weersafhankelijk aan de hand van de buitentemperatuur. Bij `Extern setpoint` regelt het systeem op een vaste temperatuur of een via Home Assistant berekend setpoint. Deze keuze bepaalt alleen de **aanvoertemperatuur**; wanneer er warmtevraag is, stel je in op [Thermostaat](./configuratie-thermostaat.html). |
| **Extern setpoint (°C)** | 15 t/m 45 °C (stap 1) | 30 °C | `number.openamber_handmatig_setpoint_cv_aanvoertemperatuur` | Gewenste cv-aanvoertemperatuur wanneer verwarmingsmodus op *Extern setpoint* staat. Kan dynamisch door Home Assistant automatiseringen worden aangepast (bijv. op basis van een slimme thermostaat of energieprijzen). Deze waarde wordt **niet** bewaard: na een herstart staat hij weer op 30 °C. Hetzelfde setpoint wordt gebruikt bij koelen met *Extern setpoint*. |
| **Verwarmen vermogen** | Beperkt / Zeer laag / Laag / Gemiddeld / Verhoogd / Hoog / Maximaal | Maximaal | `select.openamber_verwarmen_vermogen` | Begrenzing van het maximale compressorvermogen tijdens verwarmingsbedrijf. Een lagere stand verlaagt de maximale compressorfrequentie, waardoor de warmtepomp stiller draait, een hogere COP behoudt en minder snel gaat pendelen in het tussenseizoen. |

---

## Categorie: Stooklijn Z1

De stooklijn definieert de gewenste wateraanvoertemperatuur bij vijf buitentemperaturen. Tussen deze punten interpoleert OpenAmber lineair; onder -10°C en boven +15°C blijft de waarde gelijk aan het eindpunt. OpenAmber gebruikt het **gemiddelde van de buitentemperatuur over het afgelopen uur**, zodat korte schommelingen de aanvoertemperatuur niet laten springen. Op het scherm zie je de lijn ook als grafiek.

::: tip Stooklijn inregelen
Verlaag de stooklijn geleidelijk bij zacht weer (+10°C / +15°C) als het binnen te warm wordt. Verhoog bij koud weer (-10°C / 0°C) als de woning niet op temperatuur komt. Wacht na elke aanpassing minimaal 24 tot 48 uur wegens de traagheid van vloerverwarming.
:::

![Verwarmen stooklijn](/images/instellingen-verwarmen-stooklijn.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **-10°C** | 15 t/m 45 °C (stap 1) | 35 °C | `number.openamber_stooklijn_tc_10degc` | Gewenste cv-aanvoertemperatuur bij een buitentemperatuur van -10°C of lager. |
| **0°C** | 15 t/m 45 °C (stap 1) | 30 °C | `number.openamber_stooklijn_tc_0degc` | Gewenste cv-aanvoertemperatuur bij 0°C buitentemperatuur. |
| **+5°C** | 15 t/m 45 °C (stap 1) | 28 °C | `number.openamber_stooklijn_tc_5degc` | Gewenste cv-aanvoertemperatuur bij +5°C buitentemperatuur. |
| **+10°C** | 15 t/m 45 °C (stap 1) | 27 °C | `number.openamber_stooklijn_10degc` | Gewenste cv-aanvoertemperatuur bij +10°C buitentemperatuur. |
| **+15°C** | 15 t/m 45 °C (stap 1) | 25 °C | `number.openamber_stooklijn_15degc` | Gewenste cv-aanvoertemperatuur bij +15°C buitentemperatuur of hoger. |

---

## Categorie: Stooklijn Z2

Een aparte stooklijn voor zone 2. Die gebruik je als zone 2 een mengventiel heeft en een andere aanvoertemperatuur nodig heeft dan zone 1, bijvoorbeeld vloerverwarming naast radiatoren. Zet daarvoor eerst **Mengventiel zone 2 aanwezig** aan op [Algemeen](./configuratie-algemeen.html).

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **-10°C** | 15 t/m 45 °C (stap 1) | 30 °C | `number.openamber_stooklijn_zone_2_10degc_2` ¹ | Aanvoertemperatuur zone 2 bij -10°C buiten. |
| **0°C** | 15 t/m 45 °C (stap 1) | 28 °C | `number.openamber_stooklijn_zone_2_0degc` | Aanvoertemperatuur zone 2 bij 0°C buiten. |
| **+5°C** | 15 t/m 45 °C (stap 1) | 26 °C | `number.openamber_stooklijn_zone_2_5degc` | Aanvoertemperatuur zone 2 bij +5°C buiten. |
| **+10°C** | 15 t/m 45 °C (stap 1) | 24 °C | `number.openamber_stooklijn_zone_2_10degc` | Aanvoertemperatuur zone 2 bij +10°C buiten. |
| **+15°C** | 15 t/m 45 °C (stap 1) | 22 °C | `number.openamber_stooklijn_zone_2_15degc` | Aanvoertemperatuur zone 2 bij +15°C buiten. |

¹ De namen *Stooklijn zone 2 @ +10°C* en *@ -10°C* geven in Home Assistant dezelfde entity-id, omdat het `+`- en `-`-teken wegvallen. Home Assistant zet dan `_2` achter de tweede. Controleer in Home Assistant welke welke is.

---

## Categorie: Start/Stop

De start- en stopdelta bepalen de hysterese rond het actieve stooklijn- of handmatige setpoint. Deze waarden zijn cruciaal om pendelgedrag te voorkomen.

![Verwarmen start/stop](/images/instellingen-verwarmen-startstop.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Start delta (°C)** | 0.1 t/m 10.0 °C (stap 0.1) | 3.0 °C | `number.openamber_compressor_start_delta` | Aantal graden dat de watertemperatuur onder het berekende setpoint moet dalen voordat de compressor mag inschakelen (alleen als er warmtevraag is). Een kleinere waarde start sneller; een grotere waarde zorgt voor langere rustpauzes en minder starts. |
| **Stop delta (°C)** | 0.1 t/m 10.0 °C (stap 0.1) | 5.0 °C | `number.openamber_compressor_stop_delta` | Aantal graden dat de watertemperatuur boven het setpoint mag oplopen voordat de compressor stopt. Voorkomt vroegtijdige afschakeling tijdens opwarmen. Te laag veroorzaakt korte cycli; te hoog kan leiden tot oververhitting. |

---

## Categorie: Noodbedrijf

![Noodbedrijf](/images/instellingen-verwarmen-noodbedrijf.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Noodbedrijf** | Aan / Uit | Uit | `switch.openamber_noodbedrijf` | Schakelt de compressor van de buitenunit volledig uit en laat alle verwarming uitsluitend over aan het backupelement. Alleen bedoeld bij storingen, onderhoudswerkzaamheden of het testen van het backupelement. **Let op:** hoog stroomverbruik! Staat na een herstart weer uit. |
