# Troubleshooting

## The Battery Does Not Discharge

Check:

- Enable helper is on.
- SoC is above the minimum SoC.
- SoC release helper is on or SoC is above start SoC.
- Marstek user mode is `anti_feed`.
- Marstek RS485 control mode is on.
- Grid power sensor shows positive import when the house imports power.
- Calculated discharge power is above the minimum discharge power.

## The Controller Oscillates

Try:

- Increase target grid import, for example from `0 W` to `30 W`.
- Increase startup ramp duration.
- Decrease maximum ramp step.
- Increase minimum time between ramp steps.
- Use a slightly smoothed grid power sensor if your meter is noisy.

## It Exports Briefly After Startup

This can happen while Home Assistant, the Marstek integration, or the battery settles.

Use:

- Startup cooldown.
- Startup ramping.
- A small positive target grid import.

## Marstek Goes To Standby

Expected if any stop condition is active.

Common reasons:

- SoC too low.
- Enable helper off.
- User mode is not `anti_feed`.
- Sensor is unavailable.
- Calculated discharge power below minimum.

## Dynamic Charging Breaks Zero Export

Keep charging logic separate. The charging strategy should pause zero export before writing charge mode or charge power.

