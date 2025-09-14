# Home Assistant Naming Guide

Consistent names keep entities readable and easy to find.

## Entity IDs

Use the pattern `<domain>.<area>_<optional_location>_<optional_function>`.

- **domain** – entity domain such as `light`, `sensor`, `binary_sensor`, etc.
- **area** – the physical room or zone from Home Assistant.
- **optional_location** – only when multiple similar entities exist in one area (`ceiling`, `lhs`, `window`).
- **optional_function** – clarifies purpose or grouping (`group`, `temperature`, `motion`). Multi-word functions use underscores.

### Examples

`domain.area`

- `light.bedroom`
- `cover.livingroom`

`domain.area_location`

- `light.bedroom_ceiling`
- `light.bedroom_lhs`
- `cover.kitchen_window`

`domain.area_function`

- `light.bedroom_group`
- `sensor.kitchen_temperature`
- `binary_sensor.hallway_motion`

Multi-word functions:

```
binary_sensor.bedroom_contact                   # contact sensor
input_boolean.bedroom_shade_automation_enabled  # allow automation to run
input_number.bedroom_shade_night_position       # night position setting
sensor.bedroom_shade_reported_position          # current actual position
sensor.bedroom_shade_target_position            # current target position
```

## Friendly names

- Shown in the UI.
- Use natural language (`Bedroom lamp`, `Kitchen window`).
- Add simple qualifiers for duplicates (`Lamp (desk)`).

## Devices

- Device names can include brand or model (`Hue Bulb`).
- Optionally append location if a device moves (`Hue Bulb – Office`).

## Automations, scenes, helpers

- **Automations**: IDs may follow `automation.<area>_<action>` (e.g., `automation.bedroom_wake_lights`); friendly names in natural language (`Wake bedroom lights`).
- **Scenes**: `scene.<area>_<mood>` (e.g., `scene.livingroom_evening`).
- **Helpers**: use the same entity ID pattern (`input_boolean.bedroom_guest_mode`).

## Tips

- Use lowercase and underscores.
- Keep names short but descriptive.
- Rename entities when they move or change purpose.
