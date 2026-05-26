# Entity Mapping

The blueprint is entity-name agnostic. The names below are examples from one installation.

## Required Marstek Entities

| Blueprint input | Example entity |
|---|---|
| Marstek AC power sensor | `sensor.your_marstek_ac_power` |
| Marstek SoC sensor | `sensor.your_marstek_soc` |
| Marstek forced mode select | `select.your_marstek_forced_mode` |
| Marstek user mode select | `select.your_marstek_user_mode` |
| Marstek RS485 control mode switch | `switch.your_marstek_rs485_control` |
| Marstek discharge power number | `number.your_marstek_discharge_power` |

## Required Home Sensors

| Blueprint input | Example entity | Notes |
|---|---|---|
| Grid power sensor | `sensor.your_grid_power` | Positive import, negative export |
| PV power sensor | `sensor.your_pv_power` | Used for optional morning PV direct-use limiting |

## Required Helpers

| Blueprint input | Example entity |
|---|---|
| Enable helper | `input_boolean.marstek_zero_export_enabled` |
| SoC release helper | `input_boolean.marstek_zero_export_soc_released` |
| Target grid import | `input_number.marstek_zero_export_target_grid_import` |
| Maximum discharge power | `input_number.marstek_zero_export_max_discharge_power` |
| Minimum discharge power | `input_number.marstek_zero_export_min_discharge_power` |
| Minimum SoC | `input_number.marstek_zero_export_min_soc` |
| Start SoC | `input_number.marstek_zero_export_start_soc` |

## Grid Power Sign

The controller assumes:

```text
positive = grid import
negative = grid export
```

If your meter reports the opposite sign, create a template sensor:

```yaml
template:
  - sensor:
      - name: Grid Power Signed For Zero Export
        unit_of_measurement: W
        device_class: power
        state_class: measurement
        state: "{{ (states('sensor.your_grid_power') | float(0) * -1) | round(0) }}"
```
