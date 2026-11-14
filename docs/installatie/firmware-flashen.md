---
title: Firmware flashen
---

# Firmware flashen

Op deze pagina lees je welk bestand je nodig hebt en hoe je het op het board zet. Er zijn twee situaties:

| Situatie | Bestand | Hoe |
| :-- | :-- | :-- |
| **Eerste installatie**: er staat nog geen OpenAmber op het board | `….factory.bin` | Via USB, met een webflasher in de browser |
| **Update**: OpenAmber draait al | `….ota.bin` (of automatisch) | Via het scherm, Home Assistant of de webinterface, over WiFi |

## Welk bestand download je?

De firmware staat bij elke versie op de [releasepagina](https://github.com/Jordi1990/openamber/releases) onder *Assets*. Daar staan veel bestanden. Je hebt er maar **één** nodig.

**Stap 1: kies de variant die bij je board past.**

| Board | Variant (begin van de bestandsnaam) |
| :-- | :-- |
| Waveshare ESP32-S3 Touch LCD 5 inch | `openamber-esp32s3` |
| Amber Control Module (Electropaultje / leejoow) | `openamber-esp32` |
| Waveshare ESP32-P4 7 inch (1024×600) | `openamber-esp32p4` |

**Stap 2: kies het type.**

| Bestand | Wat is het? | Wanneer gebruiken? |
| :-- | :-- | :-- |
| `….factory.bin` | Het **volledige** image: bootloader, partitietabel en de firmware zelf. | Alleen voor de **eerste installatie** via USB, of om een board te herstellen dat niet meer opstart. |
| `….ota.bin` | **Alleen de firmware.** Het board moet al een bootloader en partitietabel hebben. | Voor **updates** van een board waar OpenAmber al op draait. |
| `….elf` | Debugbestand voor ontwikkelaars. | Niet nodig. |
| `….md5`, `….sha256` | Controlegetallen om te checken of een download heel is aangekomen. | Niet nodig, tenzij je de download wilt controleren. |
| `openamber.manifest.json` | Beschrijving van de release, voor de automatische update. | Niet nodig, dit gebruikt OpenAmber zelf. |

Voorbeeld: voor een eerste installatie op het Waveshare 5 inch-scherm download je `openamber-esp32s3.factory.bin`.

::: warning Niet verwisselen
Een `factory.bin` werkt **niet** als update via de webinterface, en een `ota.bin` werkt **niet** als eerste installatie via USB. Een bestand van de verkeerde variant (bijv. `esp32` op een `esp32s3`-board) start niet op.
:::

## Eerste installatie via USB

Je hebt een computer met **Chrome** of **Edge** nodig (Safari en Firefox ondersteunen geen USB-toegang vanuit de browser), en een USB-C-kabel die data doorgeeft. Sommige kabels laden alleen op.

1. Download de juiste `….factory.bin`.
2. Sluit het board met de USB-kabel aan op je computer.
3. Open [web.esphome.io](https://web.esphome.io) en klik op **Connect**. Kies de seriële poort van het board.
4. Klik op **Install**, kies het gedownloade `factory.bin`-bestand en start de installatie. Dit duurt een paar minuten.
5. Koppel het board los en sluit het opnieuw aan, of druk op reset.

Verschijnt het board niet in de lijst? Houd dan de **BOOT**-knop ingedrukt terwijl je de USB-kabel insteekt, en probeer het opnieuw.

::: tip Liever de opdrachtregel?
Met esptool kan het ook: `esptool.py write_flash 0x0 openamber-esp32s3.factory.bin`. Een factory-image schrijf je altijd naar adres `0x0`.
:::

Flash bij voorkeur voordat je het board in de regelmodule monteert.

## WiFi instellen

Na de eerste start kent OpenAmber je WiFi-netwerk nog niet. Je hebt twee mogelijkheden:

- **Op het scherm** (Waveshare): ga naar *Instellingen → WiFi*, kies je netwerk en voer het wachtwoord in.
- **Via het eigen netwerk van OpenAmber**: verbind met je telefoon of laptop met het WiFi-netwerk **OpenAmber** (wachtwoord `openamber`). Er opent een pagina waar je je eigen netwerk kiest. Opent die niet vanzelf, ga dan naar `http://192.168.4.1`.

Daarna is OpenAmber bereikbaar in je netwerk en kun je hem toevoegen aan Home Assistant (ESPHome-integratie).

## Updaten

Staat OpenAmber eenmaal op het board, dan update je **draadloos**. Je instellingen blijven bewaard.

1. **Op het scherm**: *Systeem → Beheer*. OpenAmber controleert of er een nieuwe versie is; met **Update** installeer je hem. Met **Controleer updates** zoek je opnieuw.
2. **In Home Assistant**: de entiteit **Firmware Update** (`update.openamber_firmware_update`) toont een beschikbare versie. Klik op *Installeren*.
3. **Handmatig via de webinterface**: open `http://<ip-adres-van-openamber>` in je browser. Kies onderaan bij *OTA Update* het **`ota.bin`**-bestand van jouw variant en klik op *Update*.

Bij de eerste twee manieren haalt OpenAmber de juiste versie zelf op. Je hoeft dan niets te downloaden.

::: tip Tijdens een update
OpenAmber houdt de regeling tijdens de update vast en neemt na de herstart een lopende verwarming of tapwaterlading over. Zet het board niet spanningsloos zolang het scherm *Update wordt geinstalleerd* toont.
:::

## Zelf bouwen met ESPHome

Wil je de firmware aanpassen, bijvoorbeeld met eigen sensoren? Kopieer dan de bestanden uit de map `src` van de [firmware-repository](https://github.com/Jordi1990/openamber) naar je ESPHome-configuratiemap en bouw en flash via ESPHome Builder. Je kunt dan niet meer automatisch updaten via de releases zonder je eigen wijzigingen kwijt te raken.

Volgende stap: [Hardware aansluiten](./hardware-aansluiten.html).
