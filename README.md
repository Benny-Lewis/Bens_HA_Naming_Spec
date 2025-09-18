# Home Assistant Naming Guide

Consistent names keep entities readable and easy to find.

## Entity IDs

Use the pattern `<domain>.<area>_<optional_location>_<optional_function>`.

The key is to distinguish the roles of `location` and `function`:

- **domain** – The entity domain, such as `light`, `sensor`, `binary_sensor`, etc.
- **area** – The physical room or zone from Home Assistant. This should correspond with an area defined in your HA.
- **optional_location** (The "Where/Which") – This is the **noun**. It specifies the physical place, component, or target of the entity. It answers, *"Which one is it?"* (e.g., `ceiling`, `window`, `desk`, `paddle`, `button_1`).
- **optional_function** (The "What/How") – This is the **verb or attribute**. It describes what the entity *does*, *measures*, or what *attribute* it controls. It answers, *"What does it do?"* (e.g., `motion`, `temperature`, `power`, `single_tap`, `led_color`).

### How to Apply the Naming Pattern

To create a deterministic name, apply these four rules in order. Stop at the first rule that applies.

---
#### Rule 1: The Rule of Uniqueness (Use Neither)

**IF** the `domain` and `area` are enough to uniquely and clearly identify the entity, **THEN** do not add a `location` or `function`.

* ✅ `light.office` (There is only one light switch controlling the main light in the office.)
* ✅ `media_player.living_room` (There is only one TV/media player in the living room.)

---
#### Rule 2: The Rule of Physicality (Use `location` Only)

**IF** Rule 1 does not apply because there are multiple entities of the same domain in the same area, AND they can be distinguished by their **physical location or the component they belong to**, **THEN** you **must** add a `location`.

* ✅ `light.bedroom_ceiling` and `light.bedroom_bedside`
* ✅ `switch.kitchen_island` and `switch.kitchen_countertop`

---
#### Rule 3: The Rule of Purpose (Use `function` Only)

**IF** Rule 1 does not apply, AND the entities are not distinguished by a physical location but by their **purpose, measurement, or attribute**, **THEN** you **must** add a `function`.

* ✅ `sensor.nursery_multi_sensor_temperature` and `sensor.nursery_multi_sensor_humidity`
* ✅ `input_boolean.system_guest_mode` and `input_boolean.system_away_mode`

---
#### Rule 4: The Rule of Combination (Use Both)

**IF** you must first distinguish entities by their **physical component** (Rule 2), AND then you must *further* distinguish them by their **purpose or action** (Rule 3), **THEN** you **must** use both `location` and `function`, in that order.

* ✅ `select.entryway_paddle_single_tap` and `select.entryway_paddle_double_tap`
* ✅ `sensor.front_door_lock_battery` and `sensor.front_door_lock_status`

### Examples

`domain.area`

- `light.bedroom`
- `cover.living_room`

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

- **Automations**: IDs may follow `automation.<area>_<action>` (e.g., `automation.bedroom_wake_lights`); friendly names in natural language (`Wake bedroom lights`). When the action portion gets long, break it into `<trigger>_<action_summary>` to stay scannable (e.g., `automation.entryway_btn1_press_set_home_evening`).
- **Scenes**: `scene.<area>_<mood>` (e.g., `scene.living_room_evening`).
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
  - Consider summarizing long actions as `<trigger>_<action_summary>` such as `automation.entryway_btn1_press_set_home_evening`.

`automation.entryway_paddle_double_to_entry_30_percent`
- **domain:** `automation`
- **area:** `entryway`
- **optional_location:** `paddle`
- **optional_function:** `double_to_entry_30_percent`

**Recommended gesture tokens**

- Use `pressed` for a single tap or press.
- Use `double`, `triple`, `quad`, and `quint` for multi-tap gestures.
- Use `hold` when the gesture represents a long-press.
- Use `release` for actions triggered when a button is released.

> Tip: The **optional_function** is the final part of the entity ID that describes what the entity does or measures. Area slugs themselves may include underscores (e.g., `living_room`). If you don’t need an optional function, omit it entirely—no trailing underscore.



### Annotated example: Zooz ZEN35 (paddle + four scene buttons)

Pattern: `<domain>.<area>_<optional_location>_<optional_function>`

**Load (wired chandelier)**
`light.entryway_chandelier`
- **domain:** `light`
- **area:** `entryway`
- **optional_location:** `chandelier` (The physical fixture)
- **optional_function:** *(none)*

**Controller settings (entities exposed by the device)**

`select.entryway_paddle_single_tap`
- **domain:** `select`
- **area:** `entryway`
- **optional_location:** `paddle` (The "Which")
- **optional_function:** `single_tap` (The "What")

`switch.entryway_button1_indication`
- **domain:** `switch`
- **area:** `entryway`
- **optional_location:** `button1` (The "Which")
- **optional_function:** `indication` (The "What")

`select.entryway_button1_led_color`
- **domain:** `select`
- **area:** `entryway`
- **optional_location:** `button1` (The "Which")
- **optional_function:** `led_color` (The "What")

`number.entryway_local_dimming_speed`
- **domain:** `number`
- **area:** `entryway`
- **optional_location:** *(none — applies to the whole device)*
- **optional_function:** `local_dimming_speed` (The "What")

`update.entryway_scene_controller`
- **domain:** `update`
- **area:** `entryway`
- **optional_location:** `scene_controller` (The "Which")
- **optional_function:** *(none — implied by the 'update' domain)*

`button.entryway_scene_controller_identify`
- **domain:** `button`
- **area:** `entryway`
- **optional_location:** `scene_controller` (The "Which")
- **optional_function:** `identify` (The "What")
