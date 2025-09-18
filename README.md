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

### Examples of the Rules in Action

#### Rule 1: Uniqueness (Neither)
- `light.bedroom`
- `cover.living_room`

#### Rule 2: Physicality (Location Only)
- `light.bedroom_ceiling`
- `light.bedroom_lhs` (using "left hand side" as the location)
- `cover.kitchen_window`

#### Rule 3: Purpose (Function Only)
- `light.bedroom_group`
- `sensor.kitchen_temperature`
- `binary_sensor.hallway_motion`

#### Rule 4: Combination (Both)
- `binary_sensor.bedroom_window_contact` (Location: `window`, Function: `contact`)
- `sensor.front_door_lock_battery` (Location: `lock`, Function: `battery`)


#### Complex Functions
Multi-word functions are common for helpers and template sensors. The table below shows how to parse them according to the rules.

| Entity ID | Location | Function |
| :--- | :--- | :--- |
| `binary_sensor.bedroom_contact` | *(none)* | `contact` |
| `input_boolean.bedroom_shade_automation_enabled`| `shade` | `automation_enabled` |
| `input_number.bedroom_shade_night_position` | `shade` | `night_position` |
| `sensor.bedroom_shade_reported_position` | `shade` | `reported_position` |


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

## Example: Scene controller (Zooz ZEN32)

### Annotated example: Zooz ZEN32 Scene Controller

The following entities demonstrate how the deterministic rules apply to a complex, real-world device.

---
**Load (wired chandelier) - Rule 2**
`light.entryway_chandelier`
- **domain:** `light`
- **area:** `entryway`
- **location:** `chandelier` (The "Which")
- **function:** *(none)*

---
**Controller settings (entities exposed by the device)**

**Rule 4**
`select.entryway_paddle_single_tap`
- **domain:** `select`
- **area:** `entryway`
- **location:** `paddle` (The "Which" component)
- **function:** `single_tap` (The "What" action)

**Rule 4**
`switch.entryway_button1_indication`
- **domain:** `switch`
- **area:** `entryway`
- **location:** `button1` (The "Which" component)
- **function:** `indication` (The "What" attribute)

**Rule 4**
`select.entryway_button1_led_color`
- **domain:** `select`
- **area:** `entryway`
- **location:** `button1` (The "Which" component)
- **function:** `led_color` (The "What" attribute)

**Rule 3**
`number.entryway_local_dimming_speed`
- **domain:** `number`
- **area:** `entryway`
- **location:** *(none — applies to the whole device)*
- **function:** `local_dimming_speed` (The "What" attribute)

**Rule 2**
`update.entryway_scene_controller`
- **domain:** `update`
- **area:** `entryway`
- **location:** `scene_controller` (The "Which" component)
- **function:** *(none — implied by the 'update' domain)*

**Rule 4**
`button.entryway_scene_controller_identify`
- **domain:** `button`
- **area:** `entryway`
- **location:** `scene_controller` (The "Which" component)
- **function:** `identify` (The "What" action)

---
**Automations (binding controller gestures → targets)**

`automation.entryway_btn1_pressed_to_entire_home_evening`
- **domain:** `automation`
- **area:** `entryway`
- **location:** `btn1`
- **function:** `pressed_to_entire_home_evening`
  - Consider summarizing long actions as `<trigger>_<action_summary>` such as `automation.entryway_btn1_press_set_home_evening`.

`automation.entryway_paddle_double_to_entry_30_percent`
- **domain:** `automation`
- **area:** `entryway`
- **location:** `paddle`
- **function:** `double_to_entry_30_percent`

---
**Recommended gesture tokens**

- Use `pressed` for a single tap or press.
- Use `double`, `triple`, `quad`, and `quint` for multi-tap gestures.
- Use `hold` when the gesture represents a long-press.
- Use `release` for actions triggered when a button is released.

> Tip: The **optional_function** is the final part of the entity ID that describes what the entity does or measures. Area slugs themselves may include underscores (e.g., `living_room`). If you don’t need an optional function, omit it entirely—no trailing underscore.
