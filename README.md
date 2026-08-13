[![Miele](https://img.shields.io/github/v/release/dhaucke/miele)](https://github.com/dhaucke/miele/releases/latest) [![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/hacs/integration) ![Validate with hassfest](https://github.com/dhaucke/miele/workflows/Validate%20with%20hassfest/badge.svg) [![License](https://img.shields.io/github/license/dhaucke/miele)](LICENSE)

# Miele Integration for Home Assistant

Cloud-based monitoring and control of Miele appliances for Home Assistant, with the `ecoFeedback` water/energy consumption sensors that the official core integration does not (yet) expose.

## Why this fork exists

This is a fork of [astrandb/miele](https://github.com/astrandb/miele), the original custom integration this project is built on. In May 2025 Miele support was added to Home Assistant core, and the original maintainer marked this custom component deprecated in favor of it — support for the custom version ended in January 2026.

The official core integration is a great foundation, but as of this writing it does not yet expose the `ecoFeedback` sensors this custom integration has always had:

- current water consumption per cycle
- current energy consumption per cycle
- water consumption forecast
- energy consumption forecast

If you don't need those, the [official core integration](https://home-assistant.io/integrations/miele/) is the better long-term choice — it ships with Home Assistant, needs no HACS, and is what upstream now recommends. This fork exists specifically for people who relied on the consumption sensors and don't want to lose them.

## What this integration does

The capabilities are based on Miele API version 1.0.7. The official capability overview is here: https://www.miele.com/developer/assets/API_V1.x.x_capabilities_by_device.pdf. Note that this matrix is not entirely accurate — some devices lack support the matrix claims they have, and some devices support features the matrix doesn't mark.

All supported appliances show a status sensor; some show more. Freezers, refrigerators, coffee machines, dishwashers and washer/dryers have the most complete support. Changes on the appliances are pushed to Home Assistant and shown immediately. As a backup, status is also polled from the Miele cloud every 60 seconds.

## Installation

You need Miele cloud app credentials, registered at https://developer.miele.com/get-involved. (The older `miele.com/developer` registration page has moved there — several people hit "unable to sign up" issues on the old page before finding this.)

**If the official core Miele integration is currently set up, you do not need to remove it first.** Both integrations share the `miele` domain and use the same entity `unique_id` scheme, so installing this fork via HACS and restarting Home Assistant is enough — it takes over the existing config entry in place. Your app credentials, OAuth token, devices and entity history are preserved; you will not be asked to sign in again. The one exception is the induction-hob plate-power `number` entities and the hob-plate zone-3 target-temperature sensors, which are fork-only additions with no core equivalent and will show up as new entities.

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

## Support

This is a small, unpaid fork maintained to keep one feature alive, not a funded or team-maintained project. Please don't expect the same response time as the original upstream project had.

- [Issues](https://github.com/dhaucke/miele/issues)
- Original project's [wiki](https://github.com/astrandb/miele/wiki) and [discussions](https://github.com/astrandb/miele/discussions) are still a good reference for general usage questions, since most of the underlying behavior is unchanged.

### Debugging and filing issues

If you find bugs, please download diagnostic information from the Miele integration card or the device page and attach it to your issue report.

One recurring issue is the translation of program names and phases — Miele documents this sparsely, if at all. The original maintainer built a blueprint automation that logs states from a selected sensor to the Home Assistant log to help collect this data; it still applies here: https://gist.github.com/astrandb/5ec47d6979b590639d23144142ae3100

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgist.github.com%2Fastrandb%2F5ec47d6979b590639d23144142ae3100)

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
