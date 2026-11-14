---
title: Thermostaat
---

# Thermostaat

De thermostaatinstellingen bepalen **wanneer** OpenAmber warmte- of koudevraag heeft. Hoe warm het cv-water dan wordt, regel je op de pagina's [Verwarmen](./configuratie-verwarmen.html) en [Koelen](./configuratie-koelen.html).

Er zijn twee manieren om de vraag te bepalen. Je kiest er één met **Thermostaat bron**:

| Thermostaat bron | Wie bepaalt de warmte- of koudevraag? |
| :-- | :-- |
| **Extern** (standaard) | Een bedrade kamerthermostaat op de binnenunit (aan/uit-contact) en/of de schakelaars *Externe warmtevraag* / *Externe koudevraag*, bijvoorbeeld vanuit Home Assistant. |
| **Intern** | De ingebouwde thermostaatfunctie van OpenAmber. Die vergelijkt de ruimtetemperatuur (Tr) met het setpoint, met de deadband en overrun hieronder. Gebruik deze stand ook voor de [draadloze thermostaat](#draadloze-thermostaat). |

## Hoe de warmtevraag ontstaat

OpenAmber toont het resultaat als **Warmtevraag** (`binary_sensor.openamber_warmtevraag`) en **Koudevraag** (`binary_sensor.openamber_koudevraag`). Bij twee zones is er ook *Warmtevraag Zone 1* en *Warmtevraag Zone 2*.

**Zone 1, stand Extern:** er is warmtevraag zodra **één** van deze twee aan staat:

1. het contact van de bedrade thermostaat (Modbus-register 1214), na de *uitschakelvertraging*;
2. de schakelaar **Externe warmtevraag** (`switch.openamber_externe_warmtevraag`).

**Zone 1, stand Intern:** er is warmtevraag zolang de interne thermostaat "verwarmen" aangeeft. Het bedrade contact en de schakelaar *Externe warmtevraag* worden dan genegeerd.

**Zone 2** (alleen als een mengventiel aanwezig is): er is warmtevraag als het contact voor zone 2 (register 1219, na de uitschakelvertraging) of de schakelaar **Externe warmtevraag Zone 2** aan staat. Dit geldt **in beide standen** van Thermostaat bron.

**Koelen:** gelijk aan zone 1, met het koelcontact (register 1213) en de schakelaar **Externe koudevraag**.

In alle gevallen geldt: bij een **SG Ready-blokkade** of een **storing** is er geen warmtevraag.

::: tip Wat voegt de schakelaar "Externe warmtevraag" toe?
De schakelaar is een tweede, softwarematige ingang naast het draadcontact. Je hebt hem nodig als er geen bedrade thermostaat is en de vraag uit Home Assistant komt, bijvoorbeeld van een slimme thermostaat zonder aan/uit-contact. Hij is ook handig om tijdelijk warmtevraag te forceren.

Heb je wel een bedrade thermostaat, laat de schakelaar dan uit. Staat hij aan, dan blijft er warmtevraag, ook als de thermostaat het contact opent. De uitschakelvertraging geldt niet voor de schakelaar.
:::

---

## Categorie: Algemeen

![Thermostaat algemeen](/images/instellingen-thermostaat-algemeen.jpg)
![Thermostaat algemeen pagina 2](/images/instellingen-thermostaat-algemeen-2.jpg)

De standaard setpoints zijn grijs zolang Thermostaat bron op Extern staat. De schakelaar *Draadloze thermostaat* en de koppelcode staan alleen op het Waveshare-scherm, op pagina 2 onder de uitschakelvertraging.

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Thermostaat bron** | Intern / Extern | Extern | `select.openamber_thermostaat_keuze` | Bepaalt wie de warmte- en koudevraag maakt, zie de uitleg hierboven. |
| **Standaard setpoint verwarmen** | 10.0 t/m 30.0 °C (stap 0.1) | 20.5 °C | *alleen scherm* | Ruimtesetpoint voor verwarmen dat de interne thermostaat na een herstart gebruikt. |
| **Standaard setpoint koelen** | 18.0 t/m 30.0 °C (stap 0.1) | 24.0 °C | *alleen scherm* | Ruimtesetpoint voor koelen dat de interne thermostaat na een herstart gebruikt. |
| **Uitschakelvertraging (Extern signaal)** | 0 t/m 60 min (stap 5) | 0 min | `number.openamber_uitschakelvertraging_externe_thermostaat` | Hoe lang de vraag nog actief blijft nadat het **bedrade** contact (warmte zone 1, warmte zone 2 of koude) is weggevallen. Handig bij een thermostaat die vaak kort schakelt. Geldt niet voor de schakelaars *Externe warmtevraag/koudevraag*. |
| **Draadloze thermostaat** | Aan / Uit | Uit | `switch.openamber_draadloze_thermostaat` *(standaard uitgeschakeld in HA)* | Zet de draadloze koppeling met de OpenAmber-thermostaat aan, zie hieronder. Alleen op het Waveshare-scherm. |
| **Koppelcode draadloze thermostaat** | 6 cijfers, knop *Nieuw* | — | `text_sensor.openamber_thermostaat_koppelcode` en `button.openamber_thermostaat_nieuwe_koppelcode` | De code die je op de draadloze thermostaat invoert. *Nieuw* maakt een nieuwe code; de thermostaat moet daarna opnieuw gekoppeld worden. |

De interne thermostaat zelf verschijnt in Home Assistant als klimaatentiteit **Amber thermostat** (`climate.openamber_amber_thermostat`). Daarmee stel je het actuele ruimtesetpoint en de modus in.

### Draadloze thermostaat

Op het Waveshare-scherm kan OpenAmber draadloos (ESP-NOW) koppelen met de OpenAmber-thermostaat. Die toont de ruimtetemperatuur, het setpoint en de status, en kan setpoint en modus wijzigen.

1. Zet **Thermostaat bron** op **Intern**. De thermostaat stuurt de interne thermostaat aan; in de stand Extern hebben zijn opdrachten geen effect.
2. Zet **Draadloze thermostaat** aan. OpenAmber maakt een koppelcode van 6 cijfers en toont die onder de schakelaar.
3. Voer de code in op de thermostaat.

De regeling gebruikt nog steeds de ruimtetemperatuur (Tr) van de binnenunit. De temperatuur, luchtvochtigheid en het dauwpunt van de sensor in de thermostaat komen als extra sensoren in Home Assistant (standaard uitgeschakeld).

Staat de schakelaar uit, dan draait er niets van de koppeling en wordt er niets naar het flashgeheugen geschreven.

---

## Categorie: Verwarmen

Deze instellingen gelden alleen voor de **interne** thermostaat.

![Thermostaat verwarmen](/images/instellingen-thermostaat-verwarmen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Deadband verwarmen** | 0.1 t/m 5.0 °C (stap 0.1) | 0.3 °C | *alleen scherm* | Hoeveel de ruimtetemperatuur onder het setpoint moet zakken voordat de warmtevraag start (start bij `setpoint − deadband`). |
| **Overrun verwarmen** | 0.1 t/m 5.0 °C (stap 0.1) | 0.2 °C | *alleen scherm* | Hoeveel de ruimtetemperatuur boven het setpoint mag komen voordat de warmtevraag stopt (stop bij `setpoint + overrun`). |

De interne thermostaat houdt daarnaast vaste tijden aan: minimaal 1 uur verwarmen of koelen en minimaal 15 minuten rust ertussen.

---

## Categorie: Koelen

Deze instellingen gelden alleen voor de **interne** thermostaat.

![Thermostaat koelen](/images/instellingen-thermostaat-koelen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Deadband koelen** | 0.1 t/m 5.0 °C (stap 0.1) | 0.5 °C | *alleen scherm* | Hoeveel de ruimtetemperatuur boven het koelsetpoint moet komen voordat de koudevraag start (start bij `setpoint + deadband`). |
| **Overrun koelen** | 0.1 t/m 10.0 °C (stap 0.1) | 3.0 °C | *alleen scherm* | Hoeveel de ruimtetemperatuur onder het koelsetpoint mag zakken voordat de koeling stopt (stop bij `setpoint − overrun`). |
