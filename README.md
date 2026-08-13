![Miele Integration für Home Assistant](https://raw.githubusercontent.com/dhaucke/miele/main/assets/miele-banner.svg)

# Miele

**ecoFeedback-Verbrauchsdaten für Miele-Haushaltsgeräte in Home Assistant.**

[![Release](https://img.shields.io/github/v/release/dhaucke/miele?style=flat-square)](https://github.com/dhaucke/miele/releases/latest)
[![HACS](https://img.shields.io/badge/HACS-Custom-41BDF5?style=flat-square)](https://github.com/hacs/integration)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Custom%20Integration-18BCF2?style=flat-square)](https://www.home-assistant.io/)
[![License](https://img.shields.io/github/license/dhaucke/miele?style=flat-square)](LICENSE)

**Wasserverbrauch · Energieverbrauch · Verbrauchsprognose · Füllstände · Live-Status**

[Mit HACS installieren](https://my.home-assistant.io/redirect/hacs_repository/?owner=dhaucke&repository=miele&category=integration) · [Problem melden](https://github.com/dhaucke/miele/issues)

**Sprache:** [Deutsch](#deutsch) · [English](#english)

---

# Deutsch

## Warum dieser Fork existiert

Dies ist ein Fork von [astrandb/miele](https://github.com/astrandb/miele), der ursprünglichen Custom-Integration, auf der dieses Projekt aufbaut. Im Mai 2025 wurde Miele-Unterstützung in Home Assistant Core aufgenommen, woraufhin der ursprüngliche Maintainer die Custom-Integration als deprecated markierte — der Support endete im Januar 2026.

Die offizielle Core-Integration ist eine gute Grundlage, liefert aber (Stand jetzt) nicht die `ecoFeedback`-Sensoren, die diese Custom-Integration schon immer hatte:

- aktueller Wasserverbrauch pro Zyklus
- aktueller Energieverbrauch pro Zyklus
- Wasserverbrauchsprognose
- Energieverbrauchsprognose

Wer diese Werte nicht braucht, ist mit der [offiziellen Core-Integration](https://home-assistant.io/integrations/miele/) langfristig besser bedient — sie ist Teil von Home Assistant, braucht kein HACS und ist der von Miele/Home-Assistant-Upstream empfohlene Weg. Dieser Fork existiert gezielt für alle, die auf die Verbrauchssensoren angewiesen sind und sie nicht verlieren wollen.

## Highlights

| Funktion | Beschreibung |
| --- | --- |
| **ecoFeedback-Verbrauch** | Echter, vom Gerät selbst gemessener Wasser-/Energieverbrauch pro Zyklus (nicht geschätzt) — plus separate Verbrauchsprognose |
| **Füllstände & Verbrauchsmaterial** | TwinDos, Salz, Klarspüler, PowerDisc sowie Entkalkungs-/Entfettungs-/Milchreinigungs-Zähler, je nach Gerät |
| **Live-Status** | Programm, Programmabschnitt, Restzeit, Tür, Fehler — per Server-Sent-Events sofort aktualisiert, zusätzlich per Cloud-Poll alle 60 s abgesichert |
| **Steuerung** | Ein-/Ausschalten, Start/Stopp/Pause, Zieltemperatur (Kühl-/Gefrierzonen), Lüfter, Licht — je nach Gerät und Miele-eigenen Fernsteuerungsberechtigungen |
| **Nahtlose Migration** | Gleiche `unique_id`-Schemata wie die offizielle Core-Integration — Umstieg ohne Verlust von Entity-Historie oder erneuter Anmeldung möglich |
| **Diagnose** | Home-Assistant-Diagnosedaten zur Fehlersuche |

## Was die Integration kann

Die Fähigkeiten basieren auf Miele API Version 1.0.7. Die offizielle Capability-Übersicht gibt es hier: https://www.miele.com/developer/assets/API_V1.x.x_capabilities_by_device.pdf. Diese Matrix ist nicht immer exakt — manche Geräte können weniger als angegeben, manche mehr.

Alle unterstützten Geräte zeigen mindestens einen Status-Sensor, viele deutlich mehr. Kühl-/Gefriergeräte, Kaffeevollautomaten, Spülmaschinen und Waschtrockner haben die umfangreichste Unterstützung. Änderungen am Gerät werden per Push sofort in Home Assistant angezeigt; zusätzlich wird der Status alle 60 Sekunden aus der Miele-Cloud abgefragt.

(Semi-)professionelle Gerätereihen werden von Mieles eigener 3rd-Party-API nicht unterstützt — das ist eine Einschränkung von Miele, keine dieser Integration.

## Installation

Du brauchst Miele-Cloud-App-Zugangsdaten, registrierbar unter https://developer.miele.com/get-involved. (Die ältere Seite `miele.com/developer` ist dorthin umgezogen.)

**Falls die offizielle Core-Miele-Integration bereits eingerichtet ist, musst du sie nicht vorher entfernen.** Beide Integrationen teilen sich die Domain `miele` und dasselbe Entity-`unique_id`-Schema — die Installation dieses Forks über HACS und ein Neustart genügen, der bestehende Config-Entry wird übernommen. App-Zugangsdaten, OAuth-Token, Geräte und Entity-Historie bleiben erhalten, eine erneute Anmeldung ist nicht nötig. Ausnahme: die induktionskochfeld-spezifischen `number`-Entities (Kochstufe) und der Zieltemperatur-Sensor für Zone 3 sind Fork-exklusive Ergänzungen ohne Core-Äquivalent und erscheinen als neue Entities.

### HACS (empfohlen)

1. In HACS das Drei-Punkte-Menü öffnen → **Custom repositories**.
2. `https://github.com/dhaucke/miele` als Typ **Integration** hinzufügen.
3. **Miele** installieren und Home Assistant neu starten.

### Manuell

- `custom_components/miele` aus diesem Repo in dein Home-Assistant-Verzeichnis `custom_components/miele` kopieren.
- Home Assistant neu starten.

### Einrichtung

**Einstellungen → Geräte & Dienste → Integration hinzufügen → Miele**. Eventuell muss der Browser-Cache geleert werden, damit die Integration nach der Installation gefunden wird.

Den Anweisungen folgen: zuerst die App-Zugangsdaten von https://developer.miele.com/get-involved eingeben, dann mit dem Miele-Konto anmelden und dem Home-Assistant-Client vollen Zugriff gewähren.

> **Hinweis zur Fernsteuerung:** Manche Aktionen (z. B. Einschalten/Start) sind nur verfügbar, wenn am Gerät selbst "MobileStart" bzw. die entsprechende Fernsteuerungs-Berechtigung aktiviert ist — das ist eine Miele-seitige Sicherheitseinstellung, keine Einschränkung dieser Integration.

## Support

Dies ist ein kleiner, unbezahlter Fork, der gepflegt wird, um ein Feature am Leben zu erhalten — kein finanziertes oder im Team betreutes Projekt. Bitte nicht dieselbe Reaktionszeit wie beim ursprünglichen Upstream-Projekt erwarten.

- [Issues](https://github.com/dhaucke/miele/issues)
- Das [Wiki](https://github.com/astrandb/miele/wiki) und die [Discussions](https://github.com/astrandb/miele/discussions) des Originalprojekts sind weiterhin eine gute Referenz für allgemeine Nutzungsfragen, da sich das grundlegende Verhalten größtenteils nicht geändert hat.

### Fehler melden

Bitte bei Bugs die Diagnosedaten aus der Miele-Integrationskarte oder der Geräteseite herunterladen und dem Issue anhängen.

Ein wiederkehrendes Thema ist die Übersetzung von Programmnamen und -phasen — Miele dokumentiert das kaum. Der ursprüngliche Maintainer hat dafür eine Blueprint-Automation gebaut, die Zustände eines gewählten Sensors ins Home-Assistant-Log schreibt; sie funktioniert weiterhin (hier als Kopie unter diesem Fork gehostet): https://gist.github.com/dhaucke/8b867c194e5374566d3fbe315089af00

[![Öffne deine Home-Assistant-Instanz und zeige den Blueprint-Import-Dialog mit vorausgefülltem Blueprint.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgist.github.com%2Fdhaucke%2F8b867c194e5374566d3fbe315089af00)

## Entwicklung

- Mindestens Python 3.13 wird benötigt.
- `git clone https://github.com/{dein_user}/miele && cd miele && make install_dev`
- Vor jedem Push `make lint` ausführen.

Ein VS-Code-Dev-Container ist ebenfalls eingerichtet (`.devcontainer.json`), falls das gegenüber einer lokalen Umgebung bevorzugt wird.

### Übersetzungen

Es gibt kein Lokalise-Projekt für diesen Fork. Um eine Übersetzung hinzuzufügen oder zu korrigieren, die entsprechende Datei direkt unter `custom_components/miele/translations/` bearbeiten und einen PR öffnen.

## Haftungsausschluss

Dieses Paket und sein Autor stehen in keiner Verbindung zu Miele. Nutzung auf eigene Gefahr.

## Lizenz

Veröffentlicht unter der MIT-Lizenz. Der ursprüngliche Copyright-Hinweis von [astrandb/miele](https://github.com/astrandb/miele) ist in [LICENSE](LICENSE) und in der Projekthistorie erhalten.

---

# English

## Why this fork exists

This is a fork of [astrandb/miele](https://github.com/astrandb/miele), the original custom integration this project is built on. In May 2025 Miele support was added to Home Assistant core, and the original maintainer marked this custom component deprecated in favor of it — support for the custom version ended in January 2026.

The official core integration is a great foundation, but as of this writing it does not yet expose the `ecoFeedback` sensors this custom integration has always had:

- current water consumption per cycle
- current energy consumption per cycle
- water consumption forecast
- energy consumption forecast

If you don't need those, the [official core integration](https://home-assistant.io/integrations/miele/) is the better long-term choice — it ships with Home Assistant, needs no HACS, and is what upstream now recommends. This fork exists specifically for people who relied on the consumption sensors and don't want to lose them.

## Highlights

| Feature | Description |
| --- | --- |
| **ecoFeedback consumption** | Real, appliance-measured water/energy consumption per cycle (not estimated) — plus a separate consumption forecast |
| **Filling levels & consumables** | TwinDos, salt, rinse aid, PowerDisc, plus descaling/degreasing/milk-cleaning counters, depending on the appliance |
| **Live status** | Program, program phase, remaining time, door, error - updated instantly via Server-Sent Events, backed by a 60s cloud poll |
| **Control** | Power on/off, start/stop/pause, target temperature (fridge/freezer zones), fan, light - depending on appliance and Miele's own remote-control permissions |
| **Seamless migration** | Same `unique_id` scheme as the official core integration - switch over without losing entity history or re-authenticating |
| **Diagnostics** | Home Assistant diagnostics data for troubleshooting |

## What this integration does

The capabilities are based on Miele API version 1.0.7. The official capability overview is here: https://www.miele.com/developer/assets/API_V1.x.x_capabilities_by_device.pdf. Note that this matrix is not entirely accurate — some devices lack support the matrix claims they have, and some devices support features the matrix doesn't mark.

All supported appliances show a status sensor; some show more. Freezers, refrigerators, coffee machines, dishwashers and washer/dryers have the most complete support. Changes on the appliances are pushed to Home Assistant and shown immediately. As a backup, status is also polled from the Miele cloud every 60 seconds.

(Semi-)professional appliance series are not supported by Miele's own 3rd party API — that's a Miele limitation, not one of this integration.

## Installation

You need Miele cloud app credentials, registered at https://developer.miele.com/get-involved. (The older `miele.com/developer` registration page has moved there.)

**If the official core Miele integration is currently set up, you do not need to remove it first.** Both integrations share the `miele` domain and use the same entity `unique_id` scheme, so installing this fork via HACS and restarting Home Assistant is enough — it takes over the existing config entry in place. Your app credentials, OAuth token, devices and entity history are preserved; you will not be asked to sign in again. The one exception is the induction-hob plate-power `number` entities and the hob-plate zone-3 target-temperature sensor, which are fork-only additions with no core equivalent and will show up as new entities.

### HACS (preferred)

1. In HACS, open the three-dot menu → **Custom repositories**.
2. Add `https://github.com/dhaucke/miele` as type **Integration**.
3. Install **Miele** and restart Home Assistant.

### Manual

- Copy `custom_components/miele` from this repo into your Home Assistant `custom_components/miele`.
- Restart Home Assistant.

### Setup

Go to **Settings → Devices & services → Add integration → Miele**. You may need to clear your browser cache to find it after installing.

Follow the prompts to authenticate: first enter the app credentials from https://developer.miele.com/get-involved, then sign in with your Miele account and allow full access for the Home Assistant client.

> **Remote control note:** some actions (e.g. power on/start) are only available once "MobileStart" or the equivalent remote-control permission is enabled on the appliance itself - that's a Miele-side safety setting, not a limitation of this integration.

## Support

This is a small, unpaid fork maintained to keep one feature alive, not a funded or team-maintained project. Please don't expect the same response time as the original upstream project had.

- [Issues](https://github.com/dhaucke/miele/issues)
- Original project's [wiki](https://github.com/astrandb/miele/wiki) and [discussions](https://github.com/astrandb/miele/discussions) are still a good reference for general usage questions, since most of the underlying behavior is unchanged.

### Debugging and filing issues

If you find bugs, please download diagnostic information from the Miele integration card or the device page and attach it to your issue report.

One recurring issue is the translation of program names and phases — Miele documents this sparsely, if at all. The original maintainer built a blueprint automation that logs states from a selected sensor to the Home Assistant log to help collect this data; it still applies here (mirrored under this fork): https://gist.github.com/dhaucke/8b867c194e5374566d3fbe315089af00

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgist.github.com%2Fdhaucke%2F8b867c194e5374566d3fbe315089af00)

## Development

- Make sure you have at least Python 3.13 installed.
- `git clone https://github.com/{your_user}/miele && cd miele && make install_dev`
- Run `make lint` before pushing.

A VS Code Dev Container is also set up (`.devcontainer.json`) if you prefer that over a local environment.

### Translations

There is no Lokalise project for this fork. To add or fix a translation, edit the relevant file directly under `custom_components/miele/translations/` and open a PR.

## Disclaimer

This package and its author are not affiliated with Miele. Use at your own risk.

## License

Released under the MIT license. The original copyright notice from [astrandb/miele](https://github.com/astrandb/miele) is preserved in [LICENSE](LICENSE) and in the project history.
