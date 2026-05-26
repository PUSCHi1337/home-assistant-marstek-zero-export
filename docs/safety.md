# Safety Notes

This controller writes to Marstek Modbus entities. Test carefully.

## Operating Assumptions

- Marstek user mode stays on `anti_feed`.
- Forced mode is set to `discharge` only when zero-export control is active.
- The controller sets forced discharge power, not charge power.
- Dynamic tariff charging should pause this controller before charging from grid.

## Stop Conditions

The blueprint stops discharge and sets forced mode to `standby` when:

- The enable helper is off.
- Required sensor values are unavailable.
- RS485 control mode is off.
- Marstek user mode is not the configured required mode.
- SoC is at or below the minimum SoC.
- SoC release hysteresis is not active.
- The automation is still inside startup cooldown.
- The calculated discharge power is below the minimum discharge power.

## Startup Ramp

After enabling, discharge power ramps up gradually. This avoids strong oscillations after:

- Home Assistant restart.
- Marstek restart.
- Integration reload.
- Enabling zero export while house load is already high.

## Before Restarting Home Assistant

Recommended:

1. Turn off the zero-export enable helper.
2. Turn off the zero-export automation.
3. Restart Home Assistant.
4. Wait until Marstek entities are available.
5. Re-enable the automation and helper.

## Dynamic Charging Interaction

A dynamic charging strategy should:

1. Turn off the zero-export enable helper.
2. Turn off the zero-export automation or otherwise prevent discharge writes.
3. Set Marstek forced mode to `charge`.
4. Charge as needed.
5. Return to `standby`.
6. Re-enable zero export only when charging is finished.
