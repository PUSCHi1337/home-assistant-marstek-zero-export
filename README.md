# Home Assistant Marstek Zero Export

[![release](https://img.shields.io/github/v/release/PUSCHi1337/home-assistant-marstek-zero-export?style=flat-square&label=release)](https://github.com/PUSCHi1337/home-assistant-marstek-zero-export/releases)
[![issues](https://img.shields.io/github/issues/PUSCHi1337/home-assistant-marstek-zero-export?style=flat-square&label=issues)](https://github.com/PUSCHi1337/home-assistant-marstek-zero-export/issues)
[![downloads](https://img.shields.io/github/downloads/PUSCHi1337/home-assistant-marstek-zero-export/total?style=flat-square&label=downloads)](https://github.com/PUSCHi1337/home-assistant-marstek-zero-export/releases)
[![license](https://img.shields.io/github/license/PUSCHi1337/home-assistant-marstek-zero-export?style=flat-square&label=license)](LICENSE)

Zero-export control for Marstek Venus batteries in Home Assistant.

This project provides a Home Assistant automation blueprint and optional helper package to regulate Marstek AC discharge power so the home keeps a small configurable grid import instead of exporting power.

The controller was built around the [`marstek_venus_modbus`](https://github.com/ViperRNMC/marstek_venus_modbus) integration and a grid power sensor where positive values mean grid import and negative values mean export.

## ✅ Status

- Blueprint import tested with Home Assistant `2026.5.4`.
- Disabled blueprint automation instantiation passed Home Assistant config check.
- The control strategy is field-tested on one real Marstek Venus setup.
- The reusable blueprint itself should still be treated as an early `v0.1.x` release.

## ⚠️ Safety Warning

This blueprint writes power setpoints to a battery inverter. Start with conservative values and watch the system after enabling it.

Recommended first live test:

- Maximum discharge power: `200-300 W`
- Target grid import: `30-50 W`
- Startup cooldown: `90 s`
- Startup ramp duration: `240 s`
- Maximum startup ramp step: `50-100 W`

Stop the test if RS485 control turns off, the Marstek user mode is no longer `anti_feed`, entities become unavailable, or the system exports unexpectedly for more than a short transient.

## ✨ Features

- Regulates Marstek discharge power from live grid power.
- Keeps a configurable target grid import.
- Stops at a minimum battery SoC.
- Restarts only after a configurable start SoC.
- Requires RS485 control mode to be on before writing discharge commands.
- Smooths startup ramping to avoid large oscillations after Home Assistant or battery restarts.
- Uses the Marstek number entity max limit as an additional safety cap.
- Can allow limited morning PV direct use below the normal start SoC.
- Leaves dynamic tariff and grid charging logic out of scope.

## 🛠️ Development Notes

For transparency, this project was built with AI assistance and validated against a real Home Assistant / Marstek Venus setup.

As with any automation that writes inverter setpoints, review the YAML, start with conservative limits, and test carefully in your own installation.

## 🎯 Project Scope

This repository is only for zero export / self-consumption discharge control.

Dynamic charging from cheap tariffs is intentionally separate. A later project can pause this controller, charge the battery, and then hand control back.

## 🔋 Important Marstek Notes

- For Modbus control, the Marstek user mode should stay on `anti_feed`.
- Discharging is controlled through the forced mode `discharge` and the discharge power number.
- Charging is not handled by this project.
- If Home Assistant is restarted while zero export is active, pause the controller first where possible.

## 📁 Repository Layout

```text
blueprints/automation/marstek_zero_export.yaml
packages/marstek_zero_export_helpers.yaml
dashboards/marstek_zero_export_dashboard.yaml
docs/setup.md
docs/entities.md
docs/safety.md
docs/troubleshooting.md
```

## 🚀 Quick Start

1. Install the [`marstek_venus_modbus`](https://github.com/ViperRNMC/marstek_venus_modbus) integration.
2. Create or import the helper entities from `packages/marstek_zero_export_helpers.yaml`.
3. Import the blueprint from `blueprints/automation/marstek_zero_export.yaml`.
4. Create an automation from the blueprint and map your entities.
5. Start with conservative values:
   - Target grid import: `30 W`
   - Minimum SoC: `15 %`
   - Start SoC: `20 %`
   - Minimum discharge power: `50 W`
   - Maximum discharge power: `800-1500 W`, depending on your hardware and legal setup

## 🔌 Required Semantics

Your grid power sensor must behave like this:

```text
positive = importing from grid
negative = exporting to grid
```

If your meter uses the opposite sign, create a template sensor that flips the sign before using this blueprint.

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
