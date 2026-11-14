---
title: Verwarmen instellen
---

# Verwarmen instellen

Open [Instellingen - Verwarmen](./configuratie-verwarmen.html).

## Thermostaatstrategie kiezen

Ga naar [Instellingen - Thermostaat](./configuratie-thermostaat.html) en kies:

1. `select.openamber_thermostaat_keuze` = `Intern` als OpenAmber zelf de vraag bepaalt op basis van de ruimtetemperatuur, of als je de draadloze thermostaat gebruikt.
2. `select.openamber_thermostaat_keuze` = `Extern` als een bedrade kamerthermostaat of Home Assistant (schakelaar *Externe warmtevraag*) de vraag bepaalt.
3. Gebruik je een bedrade thermostaat die vaak kort schakelt: stel eventueel een *Uitschakelvertraging* in.

## Verwarmingsmodus kiezen

1. `select.openamber_verwarmingsmodus` = `Stooklijn` voor klassieke weersafhankelijke regeling.
2. `select.openamber_verwarmingsmodus` = `Extern setpoint` voor Home Assistant gestuurde regeling.

## Start en stop delta

Stel in op [Instellingen - Verwarmen](./configuratie-verwarmen.html):

1. Start delta (`number.openamber_compressor_start_delta`).
2. Stop delta (`number.openamber_compressor_stop_delta`).

Richtlijn:

1. Te veel starts: start delta iets verhogen.
2. Te trage reactie: start delta iets verlagen.
3. Te veel doorschieten: stop delta iets verlagen.

## Stooklijn of extern setpoint

Bij stooklijn:

1. Stel alle punten in: `number.openamber_stooklijn_tc_10degc`, `number.openamber_stooklijn_tc_0degc`, `number.openamber_stooklijn_tc_5degc`, `number.openamber_stooklijn_10degc`, `number.openamber_stooklijn_15degc`.
2. Houd de lijn vloeiend en dalend.
3. Heb je een mengventiel voor zone 2, stel dan ook *Stooklijn Z2* in.

Bij extern setpoint:

1. Zet een veilige beginwaarde op `number.openamber_handmatig_setpoint_cv_aanvoertemperatuur`.
2. Laat daarna eventueel Home Assistant dynamisch sturen.

Volgende stap: [Tapwater instellen](./tapwater.html).
