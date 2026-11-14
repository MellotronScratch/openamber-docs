---
title: Algemeen
---

# Algemeen

De algemene instellingen beheren de systeemopzet van de warmtepomp, zoals de aanwezigheid van een tapwatervat, sensorkalibraties, mengventielen voor meerdere groepen, de stille modus en optionele hardware zoals de flowsensor.

::: tip Home Assistant synchronisatie
Alle onderstaande instellingen zijn via het touchscreen en via Home Assistant aanpasbaar. Wijzigingen worden direct doorgevoerd en opgeslagen.
:::

Het tabblad heeft vijf categorieën: **Systeem**, **Mengventielen**, **Sensor kalibratie**, **Stille modus** en **Opties**.

## Categorie: Systeem

![Systeem](/images/instellingen-algemeen-systeem.jpg)
![Systeem pagina 2](/images/instellingen-algemeen-systeem-2.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Tapwater aanwezig** | Aan / Uit | Uit | `switch.openamber_tapwater_aanwezig` | Geeft aan of er een tapwatervat (boiler) op het systeem is aangesloten. Schakelt de tapwaterlogica, sensoren en gerelateerde UI-tabs in of uit. Schakel dit alleen uit bij installaties die uitsluitend voor ruimteverwarming/koeling worden gebruikt. |
| **Regeltemperatuur bron** | CV-aanvoer (Tc) / CV-aanvoer zone 1 (Tv1) | CV-aanvoer (Tc) | `select.openamber_regeltemperatuur_bron` | Bepaalt welke temperatuursensor de regelaar gebruikt als actuele proceswaarde voor de PID-sturing naar het gewenste setpoint. Kies `CV-aanvoer (Tc)` voor standaardsystemen en `CV-aanvoer zone 1 (Tv1)` bij menggroepen met naregeling. |
| **Geavanceerde instellingen** | Aan / Uit | Uit | `switch.openamber_geavanceerde_instellingen` | Schakelt de weergave van geavanceerde instellingentabbladen in de UI in (zoals compressor-PID, defrost-parameters en bodemplaatverwarming). |
| **Scherm uit na** | Nooit / 30 seconden / 1 minuut / 2 minuten / 5 minuten | 1 minuut | `select.openamber_scherm_uit_na` | Na hoeveel tijd zonder aanraking het scherm uitgaat. Alleen bij het Waveshare-scherm. |
| **Analytics versturen** | Aan / Uit | Uit | `switch.openamber_analytics_versturen` | Deel anonieme prestatie- en diagnostische gegevens met het communitydashboard op [openamber.nl/devices](https://openamber.nl/devices). Gegevens worden elke 5 minuten verzonden. Daaronder staat je **Analytics ID**, waarmee je je eigen installatie op het dashboard terugvindt. |

---

## Categorie: Mengventielen

![Mengventielen](/images/instellingen-algemeen-mengventielen-1.jpg)
![Mengventielen](/images/instellingen-algemeen-mengventielen-2.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Mengventiel zone 1 aanwezig** | Aan / Uit | Uit | `switch.openamber_mengventiel_zone_1_aanwezig` | Geeft aan of een gemengde afgiftegroep (Zone 1) met motorgestuurd mengventiel aanwezig is. Activeert de automatische klepsturing en bijbehorende regellogica. |
| **Minimum positie** | 0 t/m 100% (stap 1) | 0% | `number.openamber_mengventiel_zone_1_minimum_positie` | Minimale openingsstand van mengventiel zone 1. Voorkomt dat het ventiel volledig dichtloopt en zorgt voor een gegarandeerde minimale doorstroming. |
| **Maximum positie** | 0 t/m 100% (stap 1) | 100% | `number.openamber_mengventiel_zone_1_maximum_positie` | Maximale toelaatbare openingsstand van mengventiel zone 1. |
| **Mengventiel zone 2 aanwezig** | Aan / Uit | Uit | `switch.openamber_mengventiel_zone_2_aanwezig` | Geeft aan of een tweede gemengde afgiftegroep (Zone 2) aanwezig is. Activeert mengventielsturing voor zone 2. |
| **Minimum positie** | 0 t/m 100% (stap 1) | 0% | `number.openamber_mengventiel_zone_2_minimum_positie` | Minimale openingsstand van mengventiel zone 2. |
| **Maximum positie** | 0 t/m 100% (stap 1) | 100% | `number.openamber_mengventiel_zone_2_maximum_positie` | Maximale toelaatbare openingsstand van mengventiel zone 2. |

---

## Categorie: Sensor kalibratie

Via sensorkalibratie kun je afwijkingen tussen NTC-temperatuursensoren en externe gekalibreerde thermometers compenseren.

::: tip Kalibratie-advies
Voer kalibratiewijzigingen altijd uit met kleine stappen van 0.1°C en evalueer de metingen pas na een stabiele meetperiode van minimaal 24 uur bij continu draaiende installatie.
:::

![Sensor kalibratie](/images/instellingen-algemeen-sensor-kalibratie-1.jpg)
![Sensor kalibratie](/images/instellingen-algemeen-sensor-kalibratie-2.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Ruimtetemperatuur offset** | -5.0 t/m +5.0 °C (stap 0.1) | 0.0 °C | `number.openamber_ruimte_temperatuur_offset` | Offset-correctie voor de ruimtetemperatuur (Tr) die de binnenunit meet. Deze temperatuur gebruikt de interne thermostaat. |
| **Tapwatertemperatuur offset** | -5.0 t/m +5.0 °C (stap 0.1) | 0.0 °C | `number.openamber_tapwater_temperatuur_offset` | Offset-correctie voor de dompelsensor van het tapwatervat (Tw). Controleer na een volledige opwarmcyclus. |
| **CV-aanvoertemperatuur offset** | -5.0 t/m +5.0 °C (stap 0.1) | 0.0 °C | `number.openamber_cv_aanvoertemperatuur_offset` | Offset-correctie voor de interne CV-aanvoertemperatuurmeting (Tc). |
| **CV-retourtemperatuur (Tui) offset** | -5.0 t/m +5.0 °C (stap 0.1) | 0.0 °C | `number.openamber_cv_retourtemperatuur_tui_offset` | Offset-correctie voor de retourtemperatuursensor naar de buitenunit (Tui). Essentieel voor een accurate delta-T berekening over de verdamper. |
| **CV-aanvoertemperatuur (Tuo) offset** | -5.0 t/m +5.0 °C (stap 0.1) | 0.0 °C | `number.openamber_cv_aanvoertemperatuur_tuo_offset` | Offset-correctie voor de aanvoertemperatuursensor vanaf de buitenunit (Tuo). |

---

## Categorie: Stille modus

![Stille modus](/images/instellingen-algemeen-stille-modus.jpg)

Met de stille modus beperk je het compressorvermogen binnen een tijdsvenster, bijvoorbeeld 's nachts. Dat geldt voor verwarmen, koelen en tapwater. De stille modus is alleen actief als OpenAmber de juiste tijd kent (via WiFi of Home Assistant).

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Stille modus inschakelen** | Aan / Uit | Uit | `switch.openamber_stille_modus` | Zet de stille modus aan. Of hij op dit moment actief is, zie je aan `binary_sensor.openamber_stille_modus_actief`. |
| **Starttijd** | tijd | 23:00 | `time.openamber_stille_modus_starttijd` | Begin van het venster. |
| **Eindtijd** | tijd | 07:00 | `time.openamber_stille_modus_eindtijd` | Einde van het venster. Een venster over middernacht (bijv. 23:00 tot 07:00) werkt. |
| **Stille modus vermogen** | Beperkt / Zeer laag / Laag / Gemiddeld / Verhoogd / Hoog / Maximaal | Gemiddeld | `select.openamber_stille_modus_vermogen` | Maximaal compressorvermogen binnen het venster. Staat het normale vermogen al lager, dan blijft dat gelden. |

---

## Categorie: Opties

![Opties](/images/instellingen-algemeen-opties.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Flow sensor aanwezig** | Aan / Uit | Uit | `switch.openamber_flow_sensor_aanwezig` | Schakelt de digitale flowmeter in. Hiermee berekent OpenAmber het actuele debiet (l/min), thermisch afgiftelig vermogen (kW) en realtime COP. Vereist een aangesloten flowpulssensor. |
| **Flow sensor kalibratie** | 1 t/m 1000 p/l (stap 1) | 476 p/l | `number.openamber_flow_sensor_calibration` | Aantal pulsen per liter van de gemonteerde debietmeter. De standaardwaarde van 476 p/l is nauwkeurig afgestemd op de veeltoegepaste Grundfos VFS / Sika vortex sensoren. Raadpleeg het datablad van je sensor bij een afwijkend type. |
