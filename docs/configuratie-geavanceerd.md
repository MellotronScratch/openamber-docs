---
title: Geavanceerd
---

# Geavanceerd

Het tabblad **Geavanceerd** bevat instellingen voor het fijnafstellen van de regelingen en van de buitenunit. Het heeft vijf categorieën: **PID**, **Bodemplaat**, **Defrost**, **Compressor** en **Ventilator**.

::: tip Zichtbaarheid
Dit tabblad wordt zichtbaar zodra **Geavanceerde instellingen** is ingeschakeld op de [Algemeen pagina](./configuratie-algemeen.html).
:::

::: warning Let op bij PID- en defrost-wijzigingen
De fabrieksinstellingen zijn gekozen voor typische Nederlandse woningen met vloerverwarming. Verander PID-parameters en buitenunitregisters alleen als je weet wat ze doen, en pas waarden in kleine stappen aan.
:::

## Categorie: PID

De PID-regelaars bepalen hoe snel en hoe sterk OpenAmber bijstuurt. Op het scherm staan ze verdeeld over meerdere pagina's: verwarmen, koelen, de deadbands, pomp P0 en de mengventielen.

### Wat is een PID-regelaar?

Een PID-regelaar vergelijkt steeds een **gemeten waarde** met een **gewenste waarde** (het setpoint). Het verschil daartussen heet de **afwijking**. Op basis van die afwijking rekent de regelaar uit hoe hard iets moet werken. OpenAmber gebruikt vier PID-regelaars:

| Regelaar | Meet | Gewenst | Stuurt |
| :-- | :-- | :-- | :-- |
| Compressor verwarmen | Regeltemperatuur (Tc of Tv1) | Aanvoersetpoint (stooklijn of extern) | Compressorvermogen |
| Compressor koelen | Regeltemperatuur | Koelsetpoint | Compressorvermogen |
| Pomp P0 | Delta-T over de warmtewisselaar (Tuo − Tui) | Gewenste delta-T | Pomptoerental |
| Mengventiel zone 1 / 2 | Aanvoertemperatuur van de zone | Stooklijn van de zone | Ventielstand |

De uitkomst is de optelsom van drie delen. Elk deel heeft zijn eigen instelling:

- **P, proportioneel (Kp): kijkt naar het nu.** Hoe groter de afwijking, hoe harder de regelaar stuurt. Is het water 4 °C te koud, dan stuurt hij twee keer zo hard als bij 2 °C. Een hoge Kp reageert snel, maar schiet makkelijk door. Met alleen P blijft er vaak een kleine blijvende afwijking over.
- **I, integrerend (Ki): kijkt naar het verleden.** Telt de afwijking over de tijd op. Blijft het water lang net iets te koud, dan groeit dit deel langzaam en stuurt het bij tot de afwijking weg is. Een te hoge Ki geeft schommelen: de regelaar blijft over het setpoint heen en weer gaan.
- **D, differentiërend (Kd): kijkt naar de toekomst.** Let op hoe snel de afwijking verandert. Nadert de temperatuur het setpoint snel, dan remt dit deel alvast af, zodat er minder doorschiet. Een te hoge Kd maakt de regeling onrustig bij ruis in de meting.

De **deadband** is een zone rond het setpoint. Zolang de afwijking daarbinnen blijft, verandert de uitgang nauwelijks. Zo gaat de compressor of het ventiel niet bij elke kleine schommeling bijsturen.

::: tip Vuistregels voor het afstellen
- Vloerverwarming reageert traag, in uren in plaats van minuten. Daarom zijn de standaardwaarden voor Ki laag.
- Verander **één** waarde tegelijk, in kleine stappen (bijv. 10 tot 20%), en kijk minstens een dag naar het resultaat.
- Schiet de temperatuur steeds door of schommelt hij: verlaag Ki of Kp.
- Komt de temperatuur traag of nooit op het setpoint: verhoog Kp iets, daarna eventueel Ki.
- Wisselt de compressor vaak tussen twee standen: vergroot de deadband of de [compressor hysterese](#categorie-compressor).
:::

![Verwarmen PID](/images/instellingen-geavanceerd-pid-1.jpg)

### Compressor verwarmen en koelen

De compressor-PID stuurt het vermogen van de compressor op basis van het verschil tussen de regeltemperatuur en het berekende aanvoersetpoint.

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Verwarmen PID P (Kp)** | 0.00 t/m 3.00 (stap 0.01) | 0.60 | `number.openamber_pid_verwarmen_kp` | Proportionele versterking. Hoger reageert sneller op afwijkingen, maar geeft meer kans op doorschieten; lager is rustiger maar trager. |
| **Verwarmen PID I (Ki)** | 0.0000 t/m 0.0500 (stap 0.0001) | 0.0020 | `number.openamber_pid_verwarmen_ki` | Integrale term: werkt een blijvende kleine afwijking weg. Een lage waarde past bij de trage reactie van vloerverwarming. |
| **Verwarmen PID D (Kd)** | 0.00 t/m 25.00 (stap 0.01) | 0.70 | `number.openamber_pid_verwarmen_kd` | Differentiële term: dempt snelle temperatuurschommelingen. |
| **Koelen PID P (Kp)** | 0.00 t/m 3.00 (stap 0.01) | 0.60 | `number.openamber_pid_koelen_kp` | Als bij verwarmen, voor koelen. |
| **Koelen PID I (Ki)** | 0.0000 t/m 0.0500 (stap 0.0001) | 0.0020 | `number.openamber_pid_koelen_ki` | Als bij verwarmen, voor koelen. |
| **Koelen PID D (Kd)** | 0.00 t/m 25.00 (stap 0.01) | 0.70 | `number.openamber_pid_koelen_kd` | Als bij verwarmen, voor koelen. |
| **Verwarmen PID deadband (+/-)** | 0.0 t/m 5.0 °C (stap 0.1) | 1.0 °C | `number.openamber_pid_verwarmen_deadband` | Zone rond het setpoint waarin de PID-uitgang nauwelijks verandert. Voorkomt dat de compressor bij kleine schommelingen steeds van vermogen wisselt. |
| **Koelen PID deadband (+/-)** | 0.0 t/m 5.0 °C (stap 0.1) | 1.0 °C | `number.openamber_pid_koelen_deadband` | Als bij verwarmen, voor koelen. |

### Pomp P0

Deze PID regelt het toerental van pomp P0 als **Pomp P0 Dynamisch PWM** aan staat op [Pomp](./configuratie-pomp.html). Dezelfde instellingen staan ook op dat tabblad.

![Pomp P0 PID](/images/instellingen-geavanceerd-pid-4.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Pomp P0 PID P (Kp)** | 0.00 t/m 1.00 (stap 0.01) | 0.12 | `number.openamber_pomp_p0_pid_kp` | Verhoogt het pompdebiet sneller als de delta-T (Tuo − Tui) boven de gewenste waarde komt. |
| **Pomp P0 PID I (Ki)** | 0.0000 t/m 0.0100 (stap 0.0001) | 0.0015 | `number.openamber_pomp_p0_pid_ki` | Lage integrale correctie, zodat de pomp niet gaat jagen. |
| **Pomp P0 PID D (Kd)** | 0.00 t/m 2.00 (stap 0.01) | 0.00 | `number.openamber_pomp_p0_pid_kd` | Demping bij snelle schommelingen. |

### Mengventielen

Deze PID-regelaars sturen de mengventielen van zone 1 en zone 2 naar de aanvoertemperatuur van de stooklijn van die zone. Ze werken alleen als het mengventiel aanwezig is (zie [Algemeen](./configuratie-algemeen.html)).

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Mengventiel Z1 PID P (Kp)** | 0.00 t/m 5.00 (stap 0.01) | 1.00 | `number.openamber_mengventiel_zone_1_pid_kp` | Hoe sterk het ventiel reageert op een afwijking van de aanvoertemperatuur. |
| **Mengventiel Z1 PID I (Ki)** | 0.0000 t/m 0.0500 (stap 0.0001) | 0.0050 | `number.openamber_mengventiel_zone_1_pid_ki` | Werkt een blijvende afwijking weg. |
| **Mengventiel Z1 PID D (Kd)** | 0.00 t/m 25.00 (stap 0.01) | 0.00 | `number.openamber_mengventiel_zone_1_pid_kd` | Demping. |
| **Mengventiel Z1 PID deadband (+/-)** | 0.0 t/m 5.0 °C (stap 0.1) | 1.0 °C | `number.openamber_mengventiel_zone_1_pid_deadband` | Zone rond het setpoint waarin het ventiel weinig beweegt. |
| **Mengventiel Z2 PID P/I/D, deadband** | als zone 1 | als zone 1 | `number.openamber_mengventiel_zone_2_pid_kp`, `_ki`, `_kd`, `_deadband` | Dezelfde instellingen voor zone 2. |

---

::: warning Instellingen van de buitenunit
De categorieën **Bodemplaat**, **Defrost** en **Ventilator** schrijven rechtstreeks naar registers van de buitenunit. Het scherm waarschuwt: *experimentele instellingen. Wijzigingen worden niet opgeslagen en na een power cycle teruggezet naar standaard.* Na het uit- en aanzetten van de buitenunit gelden dus weer de fabriekswaarden van de unit.

In Home Assistant staan de defrost- en ventilatorentiteiten standaard uitgeschakeld.
:::

## Categorie: Bodemplaat

De bodemplaatverwarming is een element in de lekbak van de buitenunit. Het voorkomt dat dooiwater bevriest en de afvoer blokkeert.

![Bodemplaat](/images/instellingen-geavanceerd-bodemplaat.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Bodemplaat** | Buitentemperatuur / Tijdens defrost | Tijdens defrost | `select.openamber_bodemplaat_verwarmingsmodus` | Wanneer het element aan gaat (register 3236). Bij `Tijdens defrost` gaat het aan zodra het ontdooien start en uit 2 minuten na afloop. Bij `Buitentemperatuur` gaat het aan onder de starttemperatuur hieronder. In Home Assistant staat ook de optie `Onbekend`; die hoort bij een onbekende waarde in het register. |
| **Starttemperatuur bodemplaat** | stap 1 °C | 4 °C | *alleen scherm* | Buitentemperatuur waaronder het element aan gaat in de stand `Buitentemperatuur` (register 3237). |
| **Stop-hysterese bodemplaat** | stap 1 °C | 3 °C | *alleen scherm* | Het element gaat weer uit bij `starttemperatuur + hysterese` (register 3238). |

---

## Categorie: Defrost

Deze registers bepalen wanneer de buitenunit gaat ontdooien en wanneer het ontdooien klaar is. De starttemperaturen gaan over de temperatuur van de verdamper (Tp), niet over de buitentemperatuur. Welke van de vier geldt, hangt af van de buitentemperatuur (Ta).

![Defrost](/images/instellingen-geavanceerd-defrost-1.jpg)
![Defrost](/images/instellingen-geavanceerd-defrost-2.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Exit defrost temperatuur** | stap 1 °C | 17 °C | `number.openamber_eeprom_defrost_stoptemperatuur` | Verdampertemperatuur waarbij het ontdooien klaar is (register 3277). |
| **Maximale ontdooitijd** | stap 1 min | 8 min | `number.openamber_eeprom_maximale_ontdooitijd` | Maximale duur van een ontdooicyclus (register 3278). |
| **Defrost starttemperatuur 1** | stap 1 °C | -3 °C | `number.openamber_eeprom_defrost_starttemperatuur` | Starttemperatuur bij een buitentemperatuur vanaf 3°C (register 3276). |
| **Defrost starttemperatuur 2** | -15 t/m 0 °C (stap 1) | -3 °C | `number.openamber_eeprom_defrost_starttemperatuur_2` | Starttemperatuur bij een buitentemperatuur van -3 tot 3°C (register 3336). |
| **Defrost starttemperatuur 3** | -15 t/m 0 °C (stap 1) | -5 °C | `number.openamber_eeprom_defrost_starttemperatuur_3` | Starttemperatuur bij een buitentemperatuur van -10 tot -3°C (register 3337). |
| **Defrost starttemperatuur 4** | -15 t/m 0 °C (stap 1) | -12 °C | `number.openamber_eeprom_defrost_starttemperatuur_4` | Starttemperatuur bij een buitentemperatuur onder -10°C (register 3338). |

De registers slaan temperaturen op als `T + 30`; OpenAmber rekent dat voor je om. De standaardwaarden zijn de fabriekswaarden van de buitenunit.

---

## Categorie: Compressor

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Buitenunit optimalisaties** | Aan / Uit | Aan | `switch.openamber_buitenunit_optimalisaties` | Bij elke start schrijft OpenAmber eigen frequentietabellen voor verwarmen en koelen naar de buitenunit. Ook neemt OpenAmber de begrenzing van de compressor per buitentemperatuur over van de unit. Zet dit alleen uit als je de originele tabellen van de unit wilt gebruiken. Bij HPS-units past OpenAmber deze optimalisaties nooit toe. |
| **Compressor hysterese (+/-)** | 0.1 t/m 2.0 °C (stap 0.1) | 1.0 °C | `number.openamber_compressor_hysterese` | Zolang de regeltemperatuur binnen deze afstand van het setpoint ligt, houdt de compressor zijn huidige stand vast. Dat voorkomt heen-en-weer schakelen tussen twee naburige standen. |

---

## Categorie: Ventilator

Toerentallen van de ventilator van de buitenunit, per situatie. *Ta* is de buitentemperatuur, *F4* en *F7* zijn compressorstanden. Alle waarden: 300 t/m 900 rpm, stap 10.

| Instelling | Standaard ¹ | Home Assistant entiteit | Register |
| :-- | :-- | :-- | :-- |
| **Verwarmen: koud of hoog** (Ta onder 11°C, of compressor vanaf F7) | 720 rpm | `number.openamber_eeprom_fan_verwarmen_ta_onder_11degc_of_compressor_vanaf_f7` | 3306 |
| **Verwarmen: Ta 11-17°C** | 620 rpm | `number.openamber_eeprom_fan_verwarmen_ta_11_17degc` | 3304 |
| **Verwarmen: Ta vanaf 17°C** | 520 rpm | `number.openamber_eeprom_fan_verwarmen_ta_vanaf_17degc` | 3301 |
| **Verwarmen: compressor laag** (onder F4) | 560 rpm | `number.openamber_eeprom_fan_verwarmen_compressor_onder_f4` | 3303 |
| **Verwarmen: compressor midden** (F4 tot F7) | 680 rpm | `number.openamber_eeprom_fan_verwarmen_compressor_f4_f7` | 3305 |
| **Koelen: warm of hoog** (Ta vanaf 25°C, of compressor vanaf F7) | 720 rpm | `number.openamber_eeprom_fan_koelen_ta_vanaf_25degc_of_compressor_vanaf_f7` | 3328 |
| **Koelen: Ta 14-25°C** | 720 rpm | `number.openamber_eeprom_fan_koelen_ta_14_25degc` | 3326 |
| **Koelen: Ta onder 14°C** | 620 rpm | `number.openamber_eeprom_fan_koelen_ta_onder_14degc` | 3323 |
| **Koelen: compressor laag** (onder F4) | 620 rpm | `number.openamber_eeprom_fan_koelen_compressor_onder_f4` | 3325 |
| **Koelen: compressor midden** (F4 tot F7) | 720 rpm | `number.openamber_eeprom_fan_koelen_compressor_f4_f7` | 3327 |

¹ Fabriekswaarde van de buitenunit volgens de EEPROM-tabel van OpenAmber. Je eigen unit kan afwijken; het scherm toont de waarde die nu in de unit staat.
