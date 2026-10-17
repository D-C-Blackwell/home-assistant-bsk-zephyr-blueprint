# Home Assistant BSK Zephyr Blueprint

Home Assistant automation blueprint for BSK Zephyr ventilation units exposed by the BSK Zephyr integration.

## Features

- Daily cycle-mode schedule with configurable day and night start times
- Optional weekly Schedule helper with different blocks for every day
- Per-block power, cycle, extract or intake mode and all four fan speeds
- Low fan speed during the day and night fan speed overnight
- Humidity boost using extract mode and high fan speed
- Separate boost and recovery thresholds with configurable delays
- Configurable primary humidity sensor with an optional backup sensor
- Optional away control that stops the unit or runs night speed
- Optional window contacts that power the unit off while a window is open
- Automatic recovery after Home Assistant starts or BSK entities become available

## Requirements

- A BSK Zephyr unit integrated with Home Assistant
- Power switch entity
- Fan mode select entity with `cycle` and `extract` options
- Fan speed select entity with `night`, `low` and `high` options
- One humidity sensor, either an external room sensor or the BSK sensor
- Optional second humidity sensor for fallback readings
- Optional Home Assistant Schedule helper for advanced weekly control
- Optional occupancy, person group or home zone entity
- Optional Home Assistant window contact sensors

The blueprint is designed for entities provided by the [BSK Zephyr Home Assistant integration](https://github.com/andersonshatch/hass-bskzephyr).

## Import

[![Open your Home Assistant instance and show the blueprint import dialog with this blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2FD-C-Blackwell%2Fhome-assistant-bsk-zephyr-blueprint%2Fmain%2Fblueprints%2Fautomation%2Fdc_blackwell%2Fbsk_zephyr_schedule_humidity_window.yaml)

You can also import this URL manually:

```text
https://raw.githubusercontent.com/D-C-Blackwell/home-assistant-bsk-zephyr-blueprint/main/blueprints/automation/dc_blackwell/bsk_zephyr_schedule_humidity_window.yaml
```

After importing it, create an automation from the blueprint and select the matching entities.

If the one-click button opens an empty import dialog, paste the raw URL shown above into the **Blueprint address** field. The button relies on the My Home Assistant redirect configured in the browser and cannot determine a local Home Assistant address itself.

## Weekly schedule

The simple schedule still provides configurable day and night start times. For different weekday and weekend periods, create one Home Assistant Schedule helper and select it in the blueprint.

Each active schedule block can contain the following **Additional data**:

```yaml
fan_mode: cycle
fan_speed: low
power: true
```

Supported fan modes are `cycle`, `extract` and `intake`. Supported fan speeds are `night`, `low`, `medium` and `high`. Set `power: false` for a block that should turn the unit off.

Each block can use different values. For example, a one-hour extract period can use:

```yaml
fan_mode: extract
fan_speed: high
power: true
```

The Schedule helper supports separate blocks for every day, so weekday and weekend operation can be configured without editing the blueprint. When the helper is inactive, the blueprint can use the simple day/night schedule, turn the unit off or run cycle mode at night speed.

## Behaviour

Select an external room humidity sensor as primary when one is available. An external sensor usually represents room conditions better than the sensor inside the ventilation unit. The BSK reading can change with fan direction and the air currently moving through the unit.

If no external sensor is available, select the BSK humidity sensor as the primary sensor and leave the backup field empty. If more than one suitable sensor is available, select the preferred room sensor as primary and optionally select another sensor, including the BSK sensor, as backup. The backup is used only when the primary reading is unavailable or not numeric.

Humidity at or above the boost threshold for the configured delay selects `extract` mode and `high` speed. Boost remains active until humidity reaches the recovery threshold for the configured recovery delay.

Normal low-humidity operation can follow the schedule, power the unit off or run cycle mode at night speed. If an optional occupancy entity reports that nobody is home, the unit can remain off or run at night speed. A humidity boost still starts extract mode at high speed while the home is empty.

Window control is optional. When one or more window contacts are selected, the BSK power switch is turned off if any contact is open or unavailable. Closing all contacts restores the active schedule or humidity boost. When no window sensors are selected, this control is disabled.

Set the recovery humidity below the boost humidity to prevent rapid mode changes.

## License

Licensed under the MIT License. See [LICENSE](LICENSE).
