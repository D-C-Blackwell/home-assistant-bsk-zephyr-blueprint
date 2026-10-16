# Home Assistant BSK Zephyr Blueprint

Home Assistant automation blueprint for BSK Zephyr ventilation units exposed by the BSK Zephyr integration.

## Features

- Daily cycle-mode schedule with configurable day and night start times
- Low fan speed during the day and night fan speed overnight
- Humidity boost using extract mode and high fan speed
- Separate boost and recovery thresholds with configurable delays
- Primary room humidity sensor with BSK humidity as a fallback
- Optional window contacts that power the unit off while a window is open
- Automatic recovery after Home Assistant starts or BSK entities become available

## Requirements

- A BSK Zephyr unit integrated with Home Assistant
- Power switch entity
- Fan mode select entity with `cycle` and `extract` options
- Fan speed select entity with `night`, `low` and `high` options
- Primary and backup humidity sensors
- Optional Home Assistant window contact sensors

The blueprint is designed for entities provided by the [BSK Zephyr Home Assistant integration](https://github.com/andersonshatch/hass-bskzephyr).

## Import

[![Open your Home Assistant instance and show the blueprint import dialog with this blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FD-C-Blackwell%2Fhome-assistant-bsk-zephyr-blueprint%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fdc_blackwell%2Fbsk_zephyr_schedule_humidity_window.yaml)

You can also import this URL manually:

```text
https://github.com/D-C-Blackwell/home-assistant-bsk-zephyr-blueprint/blob/main/blueprints/automation/dc_blackwell/bsk_zephyr_schedule_humidity_window.yaml
```

After importing it, create an automation from the blueprint and select the matching entities.

## Behaviour

The primary humidity sensor is used whenever it has a numeric reading. The backup sensor is used only when the primary sensor is unavailable.

Humidity at or above the boost threshold for the configured delay selects `extract` mode and `high` speed. Boost remains active until humidity reaches the recovery threshold for the configured recovery delay.

Window control is optional. When one or more window contacts are selected, the BSK power switch is turned off if any contact is open or unavailable. Closing all contacts restores the active schedule or humidity boost. When no window sensors are selected, this control is disabled.

Set the recovery humidity below the boost humidity to prevent rapid mode changes.

## License

Licensed under the MIT License. See [LICENSE](LICENSE).
