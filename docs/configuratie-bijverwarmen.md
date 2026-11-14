---
title: Bijverwarmen
---

# Bijverwarmen

De instellingen voor bijverwarmen beheren het inschakelen van een elektrische of externe bijverwarming (backupverwarming) wanneer de warmtepomp bij lage buitentemperaturen of tijdens ontdooicycli extra capaciteit nodig heeft.

::: tip Home Assistant synchronisatie
Alle onderstaande instellingen zijn via het touchscreen en via Home Assistant aanpasbaar. Wijzigingen worden direct doorgevoerd en opgeslagen.
:::

## Categorie: Algemeen

![Algemeen](/images/instellingen-bijverwarmen-algemeen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Backup element** | Intern verwarmingselement / Externe backup verwarming | Intern verwarmingselement | `select.openamber_backup_verwarmingsmodus` | Keuze tussen het ingebouwde elektrische element van de Amber of een extern aangestuurde backupverwarming (bijv. een cv-ketel in een hybride installatie). Bij `Externe backup verwarming` schakelt OpenAmber het relais *CV Hybride backup* in plaats van het interne element. |
| **Boosttemperatuur bij ontdooien** | -15 t/m 15 °C (stap 1) | -3 °C | `number.openamber_defrost_boost_buitentemperatuur` | Buitentemperatuurdrempel waaronder de backupverwarming direct na een ontdooicyclus mag inschakelen om de afkoeling van het cv-systeem snel te compenseren. |

---

## Categorie: Verwarmen

![Verwarmen](/images/instellingen-bijverwarmen-verwarmen.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Backup °min drempel** | 0 t/m 600 °C·min (stap 5) | 40 °C·min | `number.openamber_backup_element_graadminuten` | Graadminutendrempel voor het inschakelen van de backupverwarming tijdens cv-bedrijf. Zodra het opgetelde tekort tussen setpoint en actuele watertemperatuur deze drempel overschrijdt, schakelt de backup in. De actuele stand zie je in `sensor.openamber_backup_heater_degree_min_current`. Een hogere waarde bespaart stroom; een lagere waarde geeft sneller comfort. |

---

## Categorie: Tapwater

![Tapwater](/images/instellingen-bijverwarmen-tapwater.jpg)

| Instelling | Opties / Bereik | Standaard | Home Assistant entiteit | Beschrijving |
| :-- | :-- | :-- | :-- | :-- |
| **Min. verwarmsnelheid** | 0.00 t/m 5.00 °C/min (stap 0.01) | 0.12 °C/min | `number.openamber_tapwater_backup_heater_min_verwarmsnelheid` | Minimale gemiddelde stijging van de boilertemperatuur per minuut. Als de opwarming trager verloopt dan deze waarde (bijv. bij zware vorst), mag de backupverwarming bijspringen. |
| **Backupvertraging** | 0 t/m 60 min (stap 1) | 5 min | `number.openamber_tapwater_backup_heater_vertraging` | Wachttijd waarin de verwarmsnelheid continu onder de minimale drempel moet blijven voordat de backup daadwerkelijk wordt geactiveerd. Voorkomt vroegtijdig inschakelen. |
