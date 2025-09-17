# Home Assistant Naming Guide

Consistent names keep entities readable and easy to find.

## Entity IDs

Use the pattern `<domain>.<area>_<optional_location>_<optional_function>`.

- **domain** – entity domain such as `light`, `sensor`, `binary_sensor`, etc.
- **area** – the physical room or zone from Home Assistant. This should correspond with actual areas defined in your HA.
- **optional_location** – only when multiple similar entities exist in one area (`ceiling`, `lhs`, `window`).
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

Use the normal pattern for the load, and treat each button/paddle setting or gesture as the `<optional_location>` with a concise `<optional_function>`.

**Load (the wired light)**
- `light.entryway_chandelier` → “Entryway Chandelier”

**Controller settings exposed as entities**
- `select.entryway_paddle_single_tap`        → “Paddle: single-tap action”
- `select.entryway_paddle_double_tap`        → “Paddle: double-tap action”
- `switch.entryway_button1_indication`       → “Button 1 LED on/off”
- `switch.entryway_button2_indication`       → “Button 2 LED on/off”
- `switch.entryway_button3_indication`       → “Button 3 LED on/off”
- `switch.entryway_button4_indication`       → “Button 4 LED on/off”
- `select.entryway_button1_led_color`        → “Button 1 LED color”
- `select.entryway_button2_led_color`        → “Button 2 LED color”
- `select.entryway_button3_led_color`        → “Button 3 LED color”
- `select.entryway_button4_led_color`        → “Button 4 LED color”
- `number.entryway_custom_brightness_level_manual`  → “Manual custom brightness”
- `number.entryway_custom_brightness_level_zwave`   → “Z-Wave custom brightness”
- `number.entryway_local_dimming_speed`             → “Local dimming speed”
- `update.entryway_scene_controller`         → “Scene Controller Firmware”
- `button.entryway_scene_controller_identify`→ “Identify”

**Automations (binding gestures to targets)**
Pattern: `automation.<area>_<btn|paddle><index?>_<gesture>_to_<target>`
- `automation.entryway_btn1_pressed_to_entire_home_evening`
- `automation.entryway_btn2_double_to_entire_home_all_off`
- `automation.entryway_btn3_hold_to_pathway_lights`
- `automation.entryway_paddle_double_to_entry_30_percent`

**Gesture tokens (keep small and predictable)**
- `pressed` (single tap), `double`, `triple`, `quad`, `quint`, `hold`, `release`

> Why this fits the spec: `entryway` = **area**, `button1`/`paddle` = **optional_location**, and `indication`/`led_color`/`pressed` etc. = **optional_function**. Units stay out of IDs; keep them in friendly names where needed.

