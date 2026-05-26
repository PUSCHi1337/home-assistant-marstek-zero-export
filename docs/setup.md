# Setup

## Prerequisites

- Home Assistant with automation blueprints.
- A working Marstek Venus Modbus integration.
- A grid power sensor where positive values mean import and negative values mean export.
- A PV power sensor if you want to use the morning PV direct-use limiter.

## Install Helpers

Use one of these approaches:

1. Copy `packages/marstek_zero_export_helpers.yaml` into your Home Assistant packages directory.
2. Or create equivalent helpers manually in the Home Assistant UI.

If you use packages, make sure `configuration.yaml` includes a packages section:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

## Import Blueprint

Copy `blueprints/automation/marstek_zero_export.yaml` to:

```text
config/blueprints/automation/marstek_zero_export.yaml
```

Then create an automation from the blueprint and map the required entities.

## Recommended First Values

- Target grid import: `30 W`
- Minimum SoC: `15 %`
- Start SoC: `20 %`
- Minimum discharge power: `50 W`
- Maximum discharge power: start with `800 W`, then increase carefully
- Startup cooldown: `90 s`
- Startup ramp duration: `240 s`
- Maximum ramp step: `100 W`

## First Start

1. Confirm Marstek user mode is `anti_feed`.
2. Confirm RS485 control mode is on.
3. Enable the blueprint automation.
4. Turn on the zero-export helper.
5. Watch grid power, Marstek AC power, discharge setpoint, and SoC for several minutes.

