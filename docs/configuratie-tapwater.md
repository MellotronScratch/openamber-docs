---
title: Tapwater
---

# Tapwater

De instellingen voor sanitair warm water (DHW) regelen het opwarmen van het boilervat. OpenAmber biedt uitgebreide mogelijkheden voor energiezuinige opwarming, tijdschema's (bijv. voor eigen zonnestroom), winterboost en automatische legionellapreventie.

::: tip Aanbevolen inregelvolgorde
1. Stel een realistisch basiscomfort in (`setpoint` en `herstart delta`).
2. Kies het basisvermogen en stel de pompstartmodus in op delta-T.
3. Activeer desgewenst het tijdschema om overdag met zonne-energie te laden.
4. Stel de wintertemperatuurdrempel en het wintervermogen in.
5. Activeer en configureer de periodieke legionellapreventie.
:::

## Categorie: Algemeen

![Tapwater algemeen](/images/instellingen-tapwater-algemeen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Setpoint (°C)** | 25 t/m 75 °C (stap 1) | 55 °C | `number.openamber_tapwater_temperatuur_setpoint` | Doeltemperatuur voor het tapwatervat. Richtwaarde: 50 tot 55°C biedt een uitstekend compromis tussen warmwatercomfort en hoog warmtepomprendement (COP). |
| **Herstart delta (°C)** | 5 t/m 25 °C (stap 1) | 15 °C | `number.openamber_t_herstart_tapwater_verwarmen` | Aantal graden dat de boilertemperatuur onder het setpoint moet zakken voordat de warmtepomp opnieuw start met tapwaterproductie. Bij een setpoint van 55°C en delta 15°C herstart de boiler pas bij 40°C. Dit voorkomt onnodig pendelen. |
| **Basisvermogen** | Beperkt / Zeer laag / Laag / Gemiddeld / Verhoogd / Hoog / Maximaal | Beperkt | `select.openamber_tapwater_basisvermogen` | Begrenzing van het compressorvermogen tijdens normale tapwaterlading. 'Beperkt' zorgt voor rustige opwarming met lagere persgasteperaturen en maximaal rendement. |
| **Tapwaterpomp startmodus** | Aanvoer warmer dan vat (ΔT) / Samen met compressor | Aanvoer warmer dan vat (ΔT) | `select.openamber_tapwater_pomp_startmodus` | Bepaalt wanneer de tapwaterlaadpomp begint te draaien. In de delta-T modus start de pomp pas zodra de aanvoer warmer is dan het vat, waardoor afkoeling van de boiler tijdens het optoeren van de compressor wordt voorkomen. |

---

## Categorie: Schema

Met het weekschema kun je tapwaterverwarming beperken tot specifieke tijden en dagen, bijvoorbeeld om maximaal te profiteren van eigen zonnepanelen of goedkope dalurentarieven.

![Tapwater schema](/images/instellingen-tapwater-schema.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Tapwater schema** | Aan / Uit | Uit | `switch.openamber_tapwater_schema_actief` | Schakelt de schemabewaking in. Indien uitgeschakeld mag de boiler op elk moment van de dag laden zodra de temperatuur onder `setpoint - herstart delta` daalt. |
| **Ma t/m Zo** | Aan / Uit | Uit (per dag) | `switch.openamber_tapwater_schema_maandag` t/m `switch.openamber_tapwater_schema_zondag` | Bepaalt op welke dagen tapwater mag laden. **Let op:** alle dagen staan standaard uit. Zet je het schema aan zonder dagen te kiezen, dan wordt er geen tapwater meer gemaakt. Onder de dagknoppen toont het scherm *Actief op: …*. |
| **Starttijd** | 00:00 t/m 23:30 (stap 30 min op het scherm) | 10:00 | `time.openamber_tapwater_starttijd` | Begintijd van het toegestane laadvenster (bijv. 10:00 uur wanneer zonnepanelen beginnen te leveren). |
| **Eindtijd** | 00:00 t/m 23:30 (stap 30 min op het scherm) | 17:00 | `time.openamber_tapwater_eindtijd` | Eindtijd van het laadvenster. Vensters over middernacht (bijv. 22:00 tot 06:00) werken. De dagkeuze kijkt naar de huidige dag, dus na middernacht moet ook de volgende dag aan staan. Het schema werkt alleen als OpenAmber de juiste tijd kent. |

---

## Categorie: Winter

Bij lage buitentemperaturen duurt het opwarmen van tapwater langer. De wintermodus schakelt automatisch over naar een hoger compressorvermogen onder een instelbare buitentemperatuurdrempel.

![Tapwater winter](/images/instellingen-tapwater-winter.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Wintervermogen** | Gemiddeld / Verhoogd / Hoog / Maximaal | Gemiddeld | `select.openamber_tapwater_wintervermogen` | Verhoogde compressorvermogenslimiet tijdens de wintermodus om de opwarmtijd bij vriesweer binnen de perken te houden. |
| **Wintertemperatuurdrempel** | -20 t/m 10 °C (stap 1) | 5 °C | `number.openamber_buitentemperatuur_tapwater_winter_vermogen` | Buitentemperatuur waaronder de wintermodus voor tapwater actief wordt. Pas in kleine stappen aan en monitor de opwarmduur. |

---

## Categorie: Legionella

![Tapwater legionella](/images/instellingen-tapwater-legionella.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Legionellapreventie** | Aan / Uit | Uit | `switch.openamber_legionellapreventie` | Schakelt periodieke thermische desinfectie in. Aanbevolen voor huishoudens met een boilervat. |
| **Volgende legionellapreventie** | datum en tijd | 2030-01-01 10:00 | `datetime.openamber_volgende_legionellapreventie` | Moment van de eerstvolgende run. Na elke start schuift OpenAmber dit op met het ingestelde aantal dagen. Alleen in Home Assistant in te stellen, zie de waarschuwing hieronder. |
| **Legionella herhaling (dgn)** | 7 t/m 60 dagen (stap 1) | 7 d | `number.openamber_legionellapreventie_herhaling_dagen` | Interval in dagen tussen automatische legionelladesinfectie-runs. Standaard wekelijks (7 dagen). |
| **Legionella doeltemperatuur (°C)** | 55 t/m 65 °C (stap 1) | 60 °C | `number.openamber_legionellapreventie_doeltemperatuur` | Doeltemperatuur tijdens de legionellacyclus. De run is klaar zodra de boilertemperatuur (Tw) deze waarde haalt. Richtlijn: minimaal 60°C. |

::: warning Eerste legionellarun instellen
Een run start pas als het tijdstip van **Volgende legionellapreventie** voorbij is. Dat staat standaard op **1 januari 2030**. Zet het na het inschakelen in Home Assistant op het gewenste eerste moment, bijvoorbeeld zaterdag 10:00. Daarna plant OpenAmber zelf door.

Een run start daarnaast alleen als OpenAmber de juiste tijd kent en als het tapwaterschema op dat moment laden toestaat. Kies dus een tijdstip binnen je schemavenster.
:::
