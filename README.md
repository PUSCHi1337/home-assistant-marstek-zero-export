# Home Assistant Marstek Zero Export

Zero-export control for Marstek Venus batteries in Home Assistant.

This project provides a Home Assistant automation blueprint and optional helper package to regulate Marstek AC discharge power so the home keeps a small configurable grid import instead of exporting power.

The controller was built around the `marstek_venus_modbus` integration and a grid power sensor where positive values mean grid import and negative values mean export.

## Features

- Regulates Marstek discharge power from live grid power.
- Keeps a configurable target grid import.
- Stops at a minimum battery SoC.
- Restarts only after a configurable start SoC.
- Requires RS485 control mode to be on before writing discharge commands.
- Smooths startup ramping to avoid large oscillations after Home Assistant or battery restarts.
- Uses the Marstek number entity max limit as an additional safety cap.
- Can allow limited morning PV direct use below the normal start SoC.
- Leaves dynamic tariff and grid charging logic out of scope.

## Project Scope

This repository is only for zero export / self-consumption discharge control.

Dynamic charging from cheap tariffs is intentionally separate. A later project can pause this controller, charge the battery, and then hand control back.

## Important Marstek Notes

- For Modbus control, the Marstek user mode should stay on `anti_feed`.
- Discharging is controlled through the forced mode `discharge` and the discharge power number.
- Charging is not handled by this project.
- If Home Assistant is restarted while zero export is active, pause the controller first where possible.

## Repository Layout

```text
blueprints/automation/marstek_zero_export.yaml
packages/marstek_zero_export_helpers.yaml
dashboards/marstek_zero_export_dashboard.yaml
docs/setup.md
docs/entities.md
docs/safety.md
docs/troubleshooting.md
```

## Quick Start

1. Install the `marstek_venus_modbus` integration.
2. Create or import the helper entities from `packages/marstek_zero_export_helpers.yaml`.
3. Import the blueprint from `blueprints/automation/marstek_zero_export.yaml`.
4. Create an automation from the blueprint and map your entities.
5. Start with conservative values:
   - Target grid import: `30 W`
   - Minimum SoC: `15 %`
   - Start SoC: `20 %`
   - Minimum discharge power: `50 W`
   - Maximum discharge power: `800-1500 W`, depending on your hardware and legal setup

## Required Semantics

Your grid power sensor must behave like this:

```text
positive = importing from grid
negative = exporting to grid
```

If your meter uses the opposite sign, create a template sensor that flips the sign before using this blueprint.

## License

No license selected yet.
