# Home Assistant Naming Guide

Consistent names keep entities readable and easy to find.

## Entity IDs

Use the pattern `<domain>.<area>_<optional_location>_<optional_function>`.

- **domain** – entity domain such as `light`, `sensor`, `binary_sensor`, etc.
- **area** – the physical room or zone from Home Assistant. This should correspond with actual areas defined in your HA.
- **optional_location** / specifier – A specific physical location, component, or descriptor used to differentiate similar devices in the same area (e.g., `ceiling`, `window`, `desk_lamp`, `paddle`, `button1`).
- **optional_function** – included when function is not clear from **domain**. Specifies purpose or grouping (`group`, `temperature`, `motion`). Multi-word functions use underscores.

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

## Example: Scene controller (Zooz ZEN35)

### Annotated example: Zooz ZEN35 (paddle + four scene buttons)

Pattern: `<domain>.<area>_<optional_location>_<optional_function>`

**Load (wired chandelier)**
`light.entryway_chandelier`  
- **domain:** `light`  
- **area:** `entryway`  
- **optional_location:** `chandelier`  
- **optional_function:** *(none)*

**Controller settings (entities exposed by the device)**

`select.entryway_paddle_single_tap`  
- **domain:** `select`  
- **area:** `entryway`  
- **optional_location:** `paddle`  
- **optional_function:** `single_tap`

`switch.entryway_button1_indication`  
- **domain:** `switch`  
- **area:** `entryway`  
- **optional_location:** `button1`  
- **optional_function:** `indication`

`select.entryway_button1_led_color`  
- **domain:** `select`  
- **area:** `entryway`  
- **optional_location:** `button1`  
- **optional_function:** `led_color`

`number.entryway_custom_brightness_level_zwave`  
- **domain:** `number`  
- **area:** `entryway`  
- **optional_location:** `custom_brightness_level`  
- **optional_function:** `zwave`

`number.entryway_local_dimming_speed`  
- **domain:** `number`  
- **area:** `entryway`  
- **optional_location:** `local_dimming_speed`  
- **optional_function:** *(none — all meaning lives in the location)*

`update.entryway_scene_controller`  
- **domain:** `update`  
- **area:** `entryway`  
- **optional_location:** `scene_controller`  
- **optional_function:** *(none — firmware entity)*

`button.entryway_scene_controller_identify`  
- **domain:** `button`  
- **area:** `entryway`  
- **optional_location:** `scene_controller`  
- **optional_function:** `identify`

**Automations (binding controller gestures → targets)**

`automation.entryway_btn1_pressed_to_entire_home_evening`  
- **domain:** `automation`  
- **area:** `entryway`  
- **optional_location:** `btn1`  
- **optional_function:** `pressed_to_entire_home_evening`

`automation.entryway_paddle_double_to_entry_30_percent`  
- **domain:** `automation`  
- **area:** `entryway`  
- **optional_location:** `paddle`  
- **optional_function:** `double_to_entry_30_percent`

> Tip: The **optional_function** is the final part of the entity ID that describes what the entity does or measures. Area slugs themselves may include underscores (e.g., `living_room`). If you don’t need an optional function, omit it entirely—no trailing underscore.
