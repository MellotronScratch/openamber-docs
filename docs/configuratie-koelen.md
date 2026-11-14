---
title: Koelen
---

# Koelen

Met de koelfunctie kan de OpenAmber warmtepomp het afgiftesysteem (bijv. vloerverwarming of ventilatorconvectoren) voeden met koud water om het binnenklimaat in de zomer te koelen.

::: danger Condensatierisico bij vloerkoeling
Bij het koelen via vloerverwarming mag de aanvoertemperatuur nooit onder het dauwpunt zakken. Stel bij vloerkoeling de doeltemperatuur **nooit lager in dan 18°C**, tenzij een gecertificeerde dauwpuntbewaker is geïnstalleerd die de circulatie tijdig onderbreekt!
:::

## Werkwijze voor veilig inregelen

Volg bij het inregelen van de koelfunctie deze volgorde:

1. Kies de juiste koelmodus (`select.openamber_koelmodus`): intern setpoint of extern dauwpunt-gestuurd.
2. Stel het koelsetpoint veilig in (minimaal 18°C bij vloerverwarming).
3. Stem start- en stopdelta op elkaar af om pendelen te voorkomen.
4. Begrens het compressorvermogen om te snelle temperatuurschommelingen te dempen.
5. Pas eventueel de koel-PID aan in het geavanceerde menu bij overmatige schommelingen.

---

## Categorie: Algemeen

![Koelen algemeen](/images/instellingen-koelen-algemeen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Koelen vermogen** | Beperkt / Zeer laag / Laag / Gemiddeld / Verhoogd / Hoog / Maximaal | Maximaal | `select.openamber_koelen_vermogen` | Begrenzing van het maximale compressorvermogen tijdens koelen. Een te hoge stand kan leiden tot snelle onderschrijding van de condensatiegrens en korte cycli; een te lage stand geeft trage koeling. |
| **Start delta (°C)** | 0.1 t/m 10.0 °C (stap 0.1) | 3.0 °C | `number.openamber_compressor_start_delta_koelen` | Aantal graden dat de watertemperatuur boven het actieve koelsetpoint moet stijgen voordat de compressor start. |
| **Stop delta (°C)** | 0.1 t/m 10.0 °C (stap 0.1) | 5.0 °C | `number.openamber_compressor_stop_delta_koelen` | Aantal graden dat de watertemperatuur onder het koelsetpoint mag zakken voordat de compressor stopt. |

---

## Categorie: Setpoint

![Koelen setpoint](/images/instellingen-koelen-setpoint.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Koelmodus** | Fixed setpoint / Extern setpoint (in Home Assistant: Intern setpoint / Extern setpoint) | Fixed setpoint | `select.openamber_koelmodus` | Keuze tussen interne vaste temperatuurregeling of externe sturing via Home Assistant. Wanneer er koudevraag is, stel je in op [Thermostaat](./configuratie-thermostaat.html). Gebruik `Fixed setpoint` voor een eenvoudige, autonome opstelling en `Extern setpoint` voor geavanceerde regelingen (bijvoorbeeld dynamisch net boven het actuele dauwpunt). |
| **Setpoint (°C)** | 5 t/m 25 °C (stap 1) | 7 °C | `number.openamber_koelen_setpoint` | Doeltemperatuur van het koelwater bij interne koelmodus. **Waarschuwing:** De fabriekswaarde van 7°C is uitsluitend bedoeld voor fancoils. Verhoog dit bij vloerkoeling direct naar minimaal 18°C! |
| **Extern setpoint (°C)** | 15 t/m 45 °C (stap 1) | 30 °C | `number.openamber_handmatig_setpoint_cv_aanvoertemperatuur` | Actieve aanvoertemperatuur wanneer koelmodus op *Extern setpoint* staat. Wordt typisch aangestuurd via Home Assistant automatiseringen op basis van buitentemperatuur of relatieve luchtvochtigheid in de woning. Dit is **dezelfde** entiteit als het externe setpoint bij verwarmen, en de laagste waarde is 15 °C. De waarde wordt niet bewaard: na een herstart staat hij op 30 °C, laat Home Assistant hem dus opnieuw zetten. |
