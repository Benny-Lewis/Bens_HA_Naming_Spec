# Home Assistant Naming Guide

**Version:** 2.0  
**Last Updated:** 2025-10-29  
**Status:** Comprehensive Specification

Consistent, predictable naming makes your Home Assistant configuration maintainable, discoverable, and intuitive. This guide provides a deterministic naming system that scales from simple setups to complex smart homes.

---

## Table of Contents

### Getting Started
- [Quick Start Guide](#quick-start-guide)
- [5-Minute Overview](#5-minute-overview)
- [When to Use Which Pattern](#when-to-use-which-pattern)

### Core System
- [The 4-Rule Naming System](#the-4-rule-naming-system)
  - [Rule 1: Uniqueness](#rule-1-the-rule-of-uniqueness)
  - [Rule 2: Physicality](#rule-2-the-rule-of-physicality)
  - [Rule 3: Purpose](#rule-3-the-rule-of-purpose)
  - [Rule 4: Combination](#rule-4-the-rule-of-combination)
- [Pattern Template](#pattern-template)
- [Decision Tree](#decision-tree-for-entity-naming)

### Entity Types
- [Common Entity Types](#common-entity-types)
  - [Lights](#lights)
  - [Switches](#switches)
  - [Sensors](#sensors)
  - [Binary Sensors](#binary-sensors)
- [Less Common Entity Types](#less-common-entity-types)
  - [Media & Entertainment](#media--entertainment)
  - [Climate & Environment](#climate--environment)
  - [Security & Safety](#security--safety)
  - [Cleaning & Maintenance](#cleaning--maintenance)
  - [Configuration & Control](#configuration--control)
- [Helper Entities](#helper-entities)
  - [Input Booleans](#input-booleans)
  - [Input Numbers](#input-numbers)
  - [Input Selects](#input-selects)
  - [Input Text](#input-text)
  - [Input DateTime](#input-datetime)
  - [Timers](#timers)
  - [Counters](#counters)

### Automations, Scripts, and Scenes
- [Automation Naming](#automation-naming)
- [Script Naming](#script-naming)
- [Scene Naming](#scene-naming)

### Configuration Objects
- [Dashboard Naming](#dashboard-naming)
- [Blueprint Naming](#blueprint-naming)
- [Package Naming](#package-naming)
- [Template File Naming](#template-file-naming)
- [ESPHome Config Naming](#esphome-config-naming)

### Special Cases
- [Multi-Area Entities](#multi-area-entities)
- [Temporary and Seasonal Entities](#temporary-and-seasonal-entities)
- [Disabled Entities](#disabled-entities)
- [Integration-Specific Patterns](#integration-specific-patterns)
- [Virtual and Calculated Entities](#virtual-and-calculated-entities)

### Reference
- [Quick Reference Tables](#quick-reference-tables)
- [Common Location Tokens](#common-location-tokens)
- [Common Function Tokens](#common-function-tokens)
- [Pattern Summary](#pattern-summary)
- [Detailed Documentation](#detailed-documentation)

---

## Quick Start Guide

### 5-Minute Overview

**The Core Pattern:**
```
<domain>.<area>_<optional_location>_<optional_function>
```

**The 4 Rules (apply in order, stop at first match):**

1. **Uniqueness:** If domain + area is unique → use neither location nor function
   - Example: [`light.office`](naming.md:1) (only one light in office)

2. **Physicality:** If multiple entities differ by physical location → add location
   - Example: [`light.bedroom_ceiling`](naming.md:1), [`light.bedroom_bedside`](naming.md:1)

3. **Purpose:** If multiple entities differ by function/measurement → add function
   - Example: [`sensor.kitchen_temperature`](naming.md:1), [`sensor.kitchen_humidity`](naming.md:1)

4. **Combination:** If need both physical and functional distinction → use both
   - Example: [`sensor.front_door_lock_battery`](naming.md:1) (location: lock, function: battery)

**Most Common Patterns:**

| Type | Pattern | Example |
|------|---------|---------|
| Entity | `<domain>.<area>_<location>_<function>` | [`light.bedroom_ceiling`](naming.md:1) |
| Automation | `automation.<area>_<trigger>_<action>` | [`automation.bedroom_motion_light_on`](naming.md:1) |
| Script | `script.<area>_<action>_<qualifier>` | [`script.bedroom_wake_routine`](naming.md:1) |
| Scene | `scene.<area>_<mood_or_activity>` | [`scene.living_room_movie`](naming.md:1) |

**Quick Decision:**
```
START: Name my entity

1. Is domain + area unique?
   YES → Done! (e.g., light.office)
   NO → Continue

2. Multiple entities differ by location?
   YES → Add location (e.g., light.bedroom_ceiling)
   NO → Continue

3. Multiple entities differ by function?
   YES → Add function (e.g., sensor.kitchen_temperature)
   NO → Continue

4. Need both?
   YES → Add both (e.g., sensor.door_lock_battery)
```

### When to Use Which Pattern

**Use Rule 1 (Neither) when:**
- Only one entity of that type in the area
- Domain + area is self-explanatory
- Example: [`light.office`](naming.md:1), [`media_player.living_room`](naming.md:1)

**Use Rule 2 (Location) when:**
- Multiple entities of same type in area
- Distinguished by physical location
- Example: [`light.bedroom_ceiling`](naming.md:1), [`light.bedroom_bedside`](naming.md:1)

**Use Rule 3 (Function) when:**
- Multiple entities of same type in area
- Distinguished by what they measure/do
- Example: [`sensor.kitchen_temperature`](naming.md:1), [`sensor.kitchen_humidity`](naming.md:1)

**Use Rule 4 (Both) when:**
- Need to specify both location AND function
- Multiple components each with multiple functions
- Example: [`sensor.front_door_lock_battery`](naming.md:1), [`sensor.back_door_lock_battery`](naming.md:1)

---

## The 4-Rule Naming System

The deterministic naming system uses four rules applied in order. Stop at the first rule that applies.

### Pattern Template

```
<domain>.<area>_<optional_location>_<optional_function>
```

**Components:**
- **domain** – The entity domain (`light`, `sensor`, `binary_sensor`, etc.)
- **area** – The physical room or zone (should match Home Assistant areas)
- **optional_location** – The physical place or component (the "Which")
- **optional_function** – What it does or measures (the "What")

**Key Distinction:**
- **Location** (noun) = "Which one?" → `ceiling`, `window`, `desk`, `paddle`
- **Function** (verb/attribute) = "What does it do?" → `motion`, `temperature`, `power`, `single_tap`

### Rule 1: The Rule of Uniqueness

**IF** the `domain` and `area` are enough to uniquely and clearly identify the entity, **THEN** do not add a `location` or `function`.

**Examples:**
- ✅ [`light.office`](naming.md:1) - Only one light switch in office
- ✅ [`media_player.living_room`](naming.md:1) - Only one TV/media player
- ✅ [`cover.bedroom`](naming.md:1) - Only one window covering
- ✅ [`climate.home`](naming.md:1) - Only one thermostat system

**When to Apply:**
- Single entity of that domain in the area
- No ambiguity about which entity
- Domain + area is self-documenting

**When NOT to Apply:**
- Multiple entities of same domain in area
- Ambiguity about which entity
- Need to distinguish by location or function

### Rule 2: The Rule of Physicality

**IF** Rule 1 does not apply because there are multiple entities of the same domain in the same area, AND they can be distinguished by their **physical location or the component they belong to**, **THEN** you **must** add a `location`.

**Examples:**
- ✅ [`light.bedroom_ceiling`](naming.md:1) and [`light.bedroom_bedside`](naming.md:1)
- ✅ [`switch.kitchen_island`](naming.md:1) and [`switch.kitchen_countertop`](naming.md:1)
- ✅ [`cover.living_room_north`](naming.md:1) and [`cover.living_room_south`](naming.md:1)
- ✅ [`binary_sensor.bedroom_window`](naming.md:1) and [`binary_sensor.bedroom_door`](naming.md:1)

**Common Location Tokens:**
- **Furniture:** `desk`, `table`, `bedside`, `nightstand`
- **Architectural:** `ceiling`, `wall`, `floor`, `window`, `door`
- **Fixtures:** `chandelier`, `sconce`, `pendant`, `recessed`
- **Appliances:** `island`, `countertop`, `stove`, `sink`
- **Directions:** `north`, `south`, `east`, `west`, `left`, `right`
- **Components:** `paddle`, `button1`, `button2`, `relay1`

**When to Apply:**
- Multiple entities differ by physical location
- Location is the primary distinguishing factor
- Physical placement is meaningful

### Rule 3: The Rule of Purpose

**IF** Rule 1 does not apply, AND the entities are not distinguished by a physical location but by their **purpose, measurement, or attribute**, **THEN** you **must** add a `function`.

**Examples:**
- ✅ [`sensor.nursery_temperature`](naming.md:1) and [`sensor.nursery_humidity`](naming.md:1)
- ✅ [`input_boolean.guest_mode`](naming.md:1) and [`input_boolean.away_mode`](naming.md:1)
- ✅ [`binary_sensor.hallway_motion`](naming.md:1) and [`binary_sensor.hallway_occupancy`](naming.md:1)
- ✅ [`light.bedroom_group`](naming.md:1) - Virtual group of lights

**Common Function Tokens:**
- **Measurements:** `temperature`, `humidity`, `pressure`, `illuminance`, `power`
- **Detection:** `motion`, `occupancy`, `presence`, `contact`, `leak`
- **Status:** `battery`, `status`, `state`, `connection`, `signal`
- **Control:** `mode`, `preset`, `scene`, `profile`, `level`
- **Actions:** `restart`, `reboot`, `identify`, `ping`, `update`

**When to Apply:**
- Multiple entities differ by what they measure/do
- Function is the primary distinguishing factor
- Purpose is more meaningful than location

### Rule 4: The Rule of Combination

**IF** you must first distinguish entities by their **physical component** (Rule 2), AND then you must *further* distinguish them by their **purpose or action** (Rule 3), **THEN** you **must** use both `location` and `function`, in that order.

**Examples:**
- ✅ [`select.entryway_paddle_single_tap`](naming.md:1) and [`select.entryway_paddle_double_tap`](naming.md:1)
- ✅ [`sensor.front_door_lock_battery`](naming.md:1) and [`sensor.front_door_lock_status`](naming.md:1)
- ✅ [`binary_sensor.bedroom_window_contact`](naming.md:1) and [`binary_sensor.bedroom_door_contact`](naming.md:1)
- ✅ [`switch.entryway_button1_indication`](naming.md:1) and [`switch.entryway_button2_indication`](naming.md:1)

**When to Apply:**
- Need both physical AND functional distinction
- Multiple components each with multiple functions
- Location alone isn't sufficient
- Function alone isn't sufficient

**Order Matters:**
- Always: `<area>_<location>_<function>`
- Never: `<area>_<function>_<location>`

### Examples of the Rules in Action

#### Rule 1: Uniqueness (Neither)
```
light.bedroom          # Only one light control in bedroom
cover.living_room      # Only one window covering
climate.home           # Only one HVAC system
media_player.kitchen   # Only one speaker
```

#### Rule 2: Physicality (Location Only)
```
light.bedroom_ceiling      # Ceiling light
light.bedroom_bedside      # Bedside lamp
light.bedroom_closet       # Closet light

cover.kitchen_window       # Window covering
cover.kitchen_door         # Door covering
```

#### Rule 3: Purpose (Function Only)
```
sensor.kitchen_temperature     # Temperature measurement
sensor.kitchen_humidity        # Humidity measurement
sensor.kitchen_pressure        # Pressure measurement

binary_sensor.hallway_motion   # Motion detection
binary_sensor.hallway_occupancy # Occupancy detection
```

#### Rule 4: Combination (Both)
```
binary_sensor.bedroom_window_contact   # Window contact sensor
binary_sensor.bedroom_door_contact     # Door contact sensor

sensor.front_door_lock_battery         # Front door lock battery
sensor.back_door_lock_battery          # Back door lock battery

select.entryway_paddle_single_tap      # Paddle single tap action
select.entryway_paddle_double_tap      # Paddle double tap action
```

### Complex Functions

Multi-word functions are common for helpers and template sensors. Parse them according to the rules:

| Entity ID | Location | Function |
|-----------|----------|----------|
| [`binary_sensor.bedroom_contact`](naming.md:1) | *(none)* | `contact` |
| [`input_boolean.bedroom_shade_automation_enabled`](naming.md:1) | `shade` | `automation_enabled` |
| [`input_number.bedroom_shade_night_position`](naming.md:1) | `shade` | `night_position` |
| [`sensor.bedroom_shade_reported_position`](naming.md:1) | `shade` | `reported_position` |

---

## Decision Tree for Entity Naming

```
START: Name my entity

1. Is domain + area unique and clear?
   ├─ YES → DONE: Use neither location nor function
   │  Example: light.office
   └─ NO → Continue to step 2

2. Are multiple entities distinguished by physical location?
   ├─ YES → Add location token
   │  ├─ Is function also needed?
   │  │  ├─ YES → Add both (Rule 4)
   │  │  │  Example: sensor.door_lock_battery
   │  │  └─ NO → Location only (Rule 2)
   │  │     Example: light.bedroom_ceiling
   │  └─ Continue
   └─ NO → Continue to step 3

3. Are multiple entities distinguished by function/measurement?
   ├─ YES → Add function token (Rule 3)
   │  Example: sensor.kitchen_temperature
   └─ NO → Review: You may need both (Rule 4)

4. VERIFY:
   ├─ Is it unique? ✓
   ├─ Is it clear? ✓
   ├─ Does it follow pattern? ✓
   └─ Is it under 50 characters? ✓

DONE: <domain>.<area>_<location>_<function>
```

---

## Common Entity Types

These are the most frequently used entity types in Home Assistant.

### Lights

**Pattern:** `light.<area>_<location>_<function>`

**Examples:**
```
# Rule 1: Single light in area
light.office
light.bathroom
light.closet

# Rule 2: Multiple lights by location
light.bedroom_ceiling
light.bedroom_bedside
light.bedroom_closet

light.kitchen_island
light.kitchen_countertop
light.kitchen_under_cabinet

# Rule 3: Multiple lights by function
light.living_room_main
light.living_room_accent
light.living_room_reading

# Rule 4: Both location and function
light.bedroom_ceiling_main
light.bedroom_ceiling_accent
```

**Common Patterns:**
- Ceiling lights: `light.<area>_ceiling`
- Table/desk lamps: `light.<area>_<furniture>`
- Accent lighting: `light.<area>_accent`
- Under-cabinet: `light.<area>_under_cabinet`
- Outdoor: `light.frontyard_sconces`, `light.backyard_floods`

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Switches

**Pattern:** `switch.<area>_<location>_<function>`

**Examples:**
```
# Rule 1: Single switch in area
switch.garage
switch.porch

# Rule 2: Multiple switches by location
switch.kitchen_island
switch.kitchen_countertop
switch.bedroom_outlet

# Rule 3: Multiple switches by function
switch.backyard_lights
switch.backyard_fountain
switch.backyard_christmas_lights

# Rule 4: Both location and function
switch.kitchen_island_outlet
switch.kitchen_countertop_outlet
```

**Common Patterns:**
- Outlets: `switch.<area>_outlet` or `switch.<area>_<location>_outlet`
- Lights: `switch.<area>_lights` (when not using light domain)
- Appliances: `switch.<area>_<appliance>`
- Seasonal: `switch.<area>_christmas_lights`

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Sensors

**Pattern:** `sensor.<area>_<location>_<measurement>`

**Examples:**
```
# Rule 1: Single sensor type in area
sensor.bedroom_temperature
sensor.kitchen_humidity
sensor.garage_co2

# Rule 2: Multiple sensors by location
sensor.bedroom_window_temperature
sensor.bedroom_door_temperature
sensor.bedroom_closet_temperature

# Rule 3: Multiple measurements (most common)
sensor.bedroom_temperature
sensor.bedroom_humidity
sensor.bedroom_pressure
sensor.bedroom_illuminance

# Rule 4: Both location and measurement
sensor.bedroom_window_temperature
sensor.bedroom_door_temperature
```

**Common Measurements:**
- **Environmental:** `temperature`, `humidity`, `pressure`, `co2`, `voc`
- **Light:** `illuminance`, `lux`, `uv_index`
- **Energy:** `power`, `energy`, `voltage`, `current`
- **Status:** `battery`, `signal`, `rssi`, `uptime`

**Multi-Sensor Devices:**
When a single device provides multiple measurements, use function tokens:
```
sensor.bedroom_temperature    # From multi-sensor
sensor.bedroom_humidity       # From same device
sensor.bedroom_pressure       # From same device
sensor.bedroom_illuminance    # From same device
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Binary Sensors

**Pattern:** `binary_sensor.<area>_<location>_<detection_type>`

**Examples:**
```
# Rule 1: Single binary sensor in area
binary_sensor.garage_motion
binary_sensor.basement_leak

# Rule 2: Multiple sensors by location
binary_sensor.bedroom_window
binary_sensor.bedroom_door
binary_sensor.bedroom_closet_door

# Rule 3: Multiple detection types
binary_sensor.hallway_motion
binary_sensor.hallway_occupancy
binary_sensor.hallway_presence

# Rule 4: Both location and detection
binary_sensor.bedroom_window_contact
binary_sensor.bedroom_door_contact
```

**Common Detection Types:**
- **Movement:** `motion`, `occupancy`, `presence`
- **Contact:** `contact`, `door`, `window`
- **Safety:** `smoke`, `co`, `leak`, `gas`
- **Status:** `connectivity`, `problem`, `tamper`, `battery_low`

**Motion vs. Occupancy vs. Presence:**
- `motion` - Detects movement (PIR sensor)
- `occupancy` - Detects if area is occupied (may use multiple sensors)
- `presence` - Detects if specific person is present (mmWave, AI)

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

---

## Less Common Entity Types

### Media & Entertainment

#### media_player
**Pattern:** `media_player.<area>_<device>`

**Examples:**
```
media_player.living_room       # Single media player
media_player.bedroom_tv        # TV
media_player.kitchen_speaker   # Smart speaker
media_player.portable_speaker  # Portable device (no area)
```

#### camera
**Pattern:** `camera.<area>_<location>_<view>`

**Examples:**
```
camera.frontyard               # Outdoor camera
camera.doorbell_main           # Main doorbell view
camera.doorbell_package_view   # Package view
camera.backyard_clear          # High-quality stream
camera.backyard_fluent         # Low-bandwidth stream
```

#### remote
**Pattern:** `remote.<area>_<device>`

**Examples:**
```
remote.living_room_tv          # TV remote
remote.bedroom_ir              # IR blaster
remote.basement_universal      # Universal remote
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Climate & Environment

#### climate
**Pattern:** `climate.<area>_<location>`

**Examples:**
```
climate.home                   # Whole-home system
climate.upstairs               # Upstairs zone
climate.bedroom_ac             # Room AC unit
```

#### fan
**Pattern:** `fan.<area>_<type>`

**Examples:**
```
fan.bedroom_ceiling            # Ceiling fan
fan.bathroom_exhaust           # Exhaust fan
fan.office_desk                # Desk fan
```

#### weather
**Pattern:** `weather.<location>`

**Examples:**
```
weather.home                   # Home location
weather.cabin                  # Vacation home
weather.kbfi                   # Weather station
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Security & Safety

#### lock
**Pattern:** `lock.<area>_door`

**Examples:**
```
lock.front_door                # Main entry
lock.back_door                 # Rear entry
lock.garage_door               # Garage entry
lock.gate                      # Property gate
```

#### alarm_control_panel
**Pattern:** `alarm_control_panel.<area>`

**Examples:**
```
alarm_control_panel.home       # Main security system
alarm_control_panel.garage     # Garage alarm
```

#### siren
**Pattern:** `siren.<area>_<location>`

**Examples:**
```
siren.indoor                   # Indoor siren
siren.outdoor                  # Outdoor siren
siren.garage                   # Garage siren
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Cleaning & Maintenance

#### vacuum
**Pattern:** `vacuum.<area>` or `vacuum.<name>`

**Examples:**
```
vacuum.upstairs                # Upstairs robot
vacuum.downstairs              # Downstairs robot
vacuum.rosie                   # Named robot
```

#### lawn_mower
**Pattern:** `lawn_mower.<area>`

**Examples:**
```
lawn_mower.front_yard          # Front yard mower
lawn_mower.back_yard           # Back yard mower
lawn_mower.mowbot              # Named mower
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

### Configuration & Control

#### button
**Pattern:** `button.<device>_<action>`

**Examples:**
```
button.doorbell_restart        # Restart doorbell
button.router_reboot           # Reboot router
button.garage_door_open        # Open garage
```

#### select
**Pattern:** `select.<device>_<setting>`

**Examples:**
```
select.thermostat_mode         # HVAC mode
select.tv_input                # TV input
select.washer_cycle            # Washer cycle
```

#### number
**Pattern:** `number.<device>_<value>`

**Examples:**
```
number.thermostat_setpoint     # Temperature setpoint
number.motion_delay            # Motion delay
number.brightness_threshold    # Brightness threshold
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

---

## Helper Entities

Helper entities (input_* entities) are virtual entities used for automation logic and user input.

### Input Booleans

**Pattern:** `input_boolean.<purpose>_<state>`

**Examples:**
```
input_boolean.guest_mode       # Guest mode toggle
input_boolean.vacation_mode    # Vacation mode
input_boolean.quiet_hours      # Quiet hours flag
input_boolean.notify_when_away # Away notification toggle
```

**Common Patterns:**
- Mode toggles: `input_boolean.<mode>_mode`
- Feature flags: `input_boolean.<feature>_enabled`
- Status flags: `input_boolean.<system>_active`

### Input Numbers

**Pattern:** `input_number.<purpose>_<value>`

**Examples:**
```
input_number.temperature_threshold  # Temperature threshold
input_number.motion_timeout         # Motion timeout
input_number.brightness_level       # Brightness setting
```

**Common Patterns:**
- Thresholds: `input_number.<sensor>_threshold`
- Setpoints: `input_number.<device>_setpoint`
- Delays: `input_number.<automation>_delay`

### Input Selects

**Pattern:** `input_select.<purpose>_<options>`

**Examples:**
```
input_select.house_mode            # House mode selection
input_select.lighting_scene        # Lighting scene
input_select.notification_priority # Notification priority
```

**Common Patterns:**
- Mode selections: `input_select.<system>_mode`
- Scene selections: `input_select.<area>_scene`
- Profile selections: `input_select.<user>_profile`

### Input Text

**Pattern:** `input_text.<purpose>_<content>`

**Examples:**
```
input_text.alarm_message       # Alarm message
input_text.notification_text   # Notification content
input_text.guest_name          # Guest name entry
```

### Input DateTime

**Pattern:** `input_datetime.<event>_<time_aspect>`

**Examples:**
```
input_datetime.alarm_time      # Alarm time
input_datetime.vacation_start  # Vacation start date
input_datetime.reminder_datetime # Reminder timestamp
```

### Timers

**Pattern:** `timer.<purpose>_timer`

**Examples:**
```
timer.laundry_timer            # Laundry cycle timer
timer.motion_timeout           # Motion timeout
timer.cooking_timer            # Kitchen timer
```

### Counters

**Pattern:** `counter.<item>_count`

**Examples:**
```
counter.doorbell_rings         # Doorbell ring counter
counter.coffee_cups            # Coffee consumption
counter.package_deliveries     # Delivery counter
```

**See detailed guide:** [Entity Types Reference](naming/reference/entity_types_detailed.md)

---

## Automation Naming

Automations connect triggers to actions. The naming pattern reflects this trigger-action relationship.

### Core Pattern

```
automation.<area>_<trigger>_<action>
```

**Components:**
- **area:** Where the automation is triggered or primarily operates
- **trigger:** What initiates the automation (event, time, state change)
- **action:** What the automation does (turn on, notify, set scene)

### Pattern Categories

#### Time-Based Automations

**Pattern:** `automation.<area>_<time_event>_<action>`

**Examples:**
```
automation.bedroom_sunrise_lights_on
automation.exterior_sunset_lights_on
automation.home_midnight_lights_off
automation.living_room_evening_scene_cozy
```

**Common Time Triggers:**
- `sunrise`, `sunset`, `dawn`, `dusk`
- `morning`, `evening`, `night`, `midnight`
- `<time>` (e.g., `0700`, `2200`)

#### Event-Based Automations

**Pattern:** `automation.<area>_<event>_<action>`

**Examples:**
```
automation.bedroom_motion_light_on
automation.front_door_opened_notify
automation.garage_door_closed_lock
automation.doorbell_pressed_notify
```

**Common Event Triggers:**
- Motion: `motion`, `motion_detected`, `motion_cleared`
- Contact: `opened`, `closed`, `door_opened`, `window_opened`
- Button: `pressed`, `double_pressed`, `held`
- State: `on`, `off`, `home`, `away`

#### Button/Controller Automations

**Pattern:** `automation.<area>_<button>_<gesture>_<action>`

**Examples:**
```
automation.entryway_btn1_pressed_scene_evening
automation.entryway_btn2_pressed_scene_tv
automation.entryway_btn12_double_scene_bright
automation.living_room_pico_on_pressed_lights_on
```

**Gesture Tokens:**
- `pressed` - Single tap/press
- `double` - Double tap
- `triple` - Triple tap
- `held` - Long press
- `released` - Button released

#### Multi-Area Automations

**Pattern:** `automation.<scope>_<trigger>_<action>`

**Examples:**
```
automation.home_away_mode_lights_off
automation.exterior_sunset_lights_on
automation.upstairs_bedtime_lights_dim
```

**Scope Tokens:**
- `home` - Whole home
- `upstairs`/`downstairs` - Floor level
- `exterior` - All outdoor areas
- `interior` - All indoor areas

### Automation Naming Examples

**✅ Good Examples:**
```
automation.bedroom_motion_light_on
automation.kitchen_sunset_lights_on
automation.front_door_opened_notify
automation.entryway_btn1_pressed_scene_evening
automation.home_away_mode_all_off
```

**❌ Anti-Patterns:**
```
automation.turn_on_bedroom_light_when_motion  # Too verbose
automation.automation1                         # Not descriptive
automation.bedroom_light                       # Missing trigger
automation.motion_detected                     # Missing area and action
```

### Decision Tree for Automation Naming

```
START: Name my automation

1. IDENTIFY TRIGGER AREA
   ├─ Single room? → Use room name
   ├─ Whole home? → Use "home"
   ├─ Floor/level? → Use floor name
   └─ Exterior? → Use "exterior"

2. IDENTIFY TRIGGER TYPE
   ├─ Time-based? → Use time event (sunset, 0700, etc.)
   ├─ Motion? → Use "motion"
   ├─ Button? → Use button identifier (btn1, pico_on, etc.)
   ├─ State change? → Use state (opened, closed, home, away)
   └─ Other event? → Use descriptive trigger

3. IDENTIFY ACTION
   ├─ Control lights? → lights_on, lights_off, lights_dim
   ├─ Activate scene? → scene_<scene_name>
   ├─ Send notification? → notify
   ├─ Run script? → script_<script_name>
   └─ Other? → Use descriptive action

4. COMBINE
   automation.<area>_<trigger>_<action>

5. VERIFY
   ├─ Under 60 characters? ✓
   ├─ Clear trigger? ✓
   ├─ Clear action? ✓
   └─ Follows pattern? ✓

DONE: automation.bedroom_motion_light_on
```

**See detailed guide:** [Automation Naming Reference](naming/reference/automations_detailed.md)

---

## Script Naming

Scripts are reusable sequences of actions. The naming pattern emphasizes the action being performed.

### Core Pattern

```
script.<area>_<action>_<qualifier>
```

**Components:**
- **area:** Where the script operates (or `home` for multi-area)
- **action:** What the script does (verb-based)
- **qualifier:** Optional additional context

### Pattern Categories

#### Reusable Scripts

**Pattern:** `script.<area>_<action>`

**Examples:**
```
script.bedroom_wake_routine
script.kitchen_cleanup_lights
script.home_goodnight_sequence
script.exterior_security_check
```

#### One-Off Scripts

**Pattern:** `script.<area>_<specific_action>`

**Examples:**
```
script.living_room_movie_mode
script.bedroom_reading_light
script.kitchen_dinner_prep
```

#### Timed Scripts

**Pattern:** `script.<area>_<action>_<duration>`

**Examples:**
```
script.bathroom_fan_15min
script.garage_light_5min
script.porch_light_30min
```

#### Data Management Scripts

**Pattern:** `script.<system>_<action>_<data>`

**Examples:**
```
script.system_backup_config
script.system_cleanup_logs
script.system_export_data
```

### Scripts vs. Automations

**Use Script When:**
- Action sequence is reusable
- Called by multiple automations
- Manual execution needed
- Complex logic or sequences
- Parameterized actions

**Use Automation When:**
- Triggered by specific event
- One-time use case
- Simple trigger-action pair
- No reusability needed

**Example:**
```yaml
# Script: Reusable wake routine
script.bedroom_wake_routine:
  sequence:
    - service: light.turn_on
      target:
        entity_id: light.bedroom_ceiling
      data:
        brightness: 255
    - service: cover.open_cover
      target:
        entity_id: cover.bedroom
    - delay: 00:00:30
    - service: media_player.play_media
      # ...

# Automation: Triggers the script
automation.bedroom_alarm_wake_routine:
  trigger:
    - platform: time
      at: input_datetime.alarm_time
  action:
    - service: script.turn_on
      target:
        entity_id: script.bedroom_wake_routine
```

### Script Naming Examples

**✅ Good Examples:**
```
script.bedroom_wake_routine
script.home_goodnight_sequence
script.kitchen_cleanup_lights
script.bathroom_fan_15min
script.system_backup_config
```

**❌ Anti-Patterns:**
```
script.script1                 # Not descriptive
script.do_bedroom_stuff        # Too vague
script.turn_on_lights          # Missing area
script.bedroom                 # Missing action
```

**See detailed guide:** [Script Naming Reference](naming/reference/scripts_detailed.md)

---

## Scene Naming

Scenes capture a desired state for multiple entities, optimized for a specific purpose, mood, or time of day.

### Core Pattern

```
scene.<area>_<mood_or_activity>
```

**Components:**
- **area:** The scope of the scene (room, floor, or home)
- **mood_or_activity:** The purpose, mood, or activity

### Scope Selection

**Single-Area Scenes:**
```
scene.bedroom_bright
scene.bedroom_reading
scene.living_room_movie
scene.kitchen_cooking
```

**Multi-Area Scenes:**
```
scene.upstairs_bright          # Entire floor
scene.exterior_evening         # All outdoor
scene.home_away                # Whole home
```

### Purpose Types

#### Mood-Based (Brightness)
```
scene.bedroom_bright           # Maximum brightness
scene.bedroom_medium           # Medium brightness
scene.bedroom_dim              # Low brightness
scene.bedroom_night            # Very low/night light
```

#### Activity-Based
```
scene.living_room_movie        # Movie watching
scene.living_room_tv           # TV watching
scene.bedroom_reading          # Reading
scene.kitchen_cooking          # Cooking
scene.office_work              # Working
```

#### Time-Based
```
scene.home_morning             # Morning routine
scene.home_evening             # Evening ambiance
scene.bedroom_bedtime          # Before sleep
scene.kitchen_morning          # Morning kitchen
```

#### State-Based
```
scene.home_away                # Leaving home
scene.home_sleep               # Sleeping
scene.home_all_off             # Everything off
scene.home_all_on              # Everything on
```

### Scene Naming Examples

**✅ Good Examples:**
```
scene.bedroom_bright
scene.living_room_movie
scene.home_evening
scene.exterior_night
scene.upstairs_dim
```

**❌ Anti-Patterns:**
```
scene.bright                   # Missing area
scene.tvnew_scene              # Poor naming, redundant "scene"
scene.bedroom_scene_1          # Not descriptive
scene.upstairs_bright          # Verify scope! (if affects downstairs too, use "home")
```

### Important: Verify Scope

**Critical:** Ensure the area name accurately reflects which entities are affected.

```
❌ Bad: scene.upstairs_bright (but affects downstairs too)
✅ Good: scene.home_bright (accurately reflects whole-home scope)
```

**See detailed guide:** [Scene Naming Reference](naming/reference/scenes_detailed.md)

---

## Dashboard Naming

Dashboards have multiple naming components: slug (URL), title (display), icon, and filename.

### Dashboard Slug

**Pattern:** `<purpose>-dashboard` (kebab-case)

**Examples:**
```yaml
humidity-dashboard:
  title: "Humidity"
  icon: mdi:water-percent
  filename: dashboards/humidity_dashboard.yaml

security-overview:
  title: "Security"
  icon: mdi:shield-home
  filename: dashboards/security_overview.yaml
```

### Dashboard File Naming

**Pattern:** `dashboards/<purpose>_dashboard.yaml` (snake_case)

**Examples:**
```
dashboards/humidity_dashboard.yaml
dashboards/security_overview.yaml
dashboards/energy_monitoring.yaml
dashboards/bedroom_controls.yaml
```

**See detailed guide:** [Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)

---

## Blueprint Naming

Blueprints are reusable automation/script/template patterns.

### Blueprint File Path

**Pattern:** `blueprints/<type>/<source>/<name>.yaml`

**Examples:**
```
blueprints/automation/homeassistant/motion_light.yaml
blueprints/automation/custom/bedroom_wake_routine.yaml
blueprints/script/homeassistant/confirmable_notification.yaml
blueprints/template/homeassistant/inverted_binary_sensor.yaml
```

**See detailed guide:** [Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)

---

## Package Naming

Packages allow modular organization of configuration.

### Package File Naming

**Pattern:** `packages/<scope>.yaml`

**Examples:**
```
packages/climate.yaml          # Climate control
packages/security.yaml         # Security system
packages/living_room.yaml      # Living room config
packages/lutron.yaml           # Lutron integration
```

**See detailed guide:** [Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)

---

## Template File Naming

Template files contain template entity definitions.

### Template File Naming

**Pattern:** `templates/<purpose>.yaml`

**Examples:**
```
templates/sensor_templates.yaml
templates/calculations.yaml
templates/climate_templates.yaml
templates/examples.yaml
```

**See detailed guide:** [Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)

---

## ESPHome Config Naming

ESPHome device configurations define individual ESP devices.

### ESPHome File Naming

**Pattern:** `esphome/<device-type>-<identifier>.yaml` (kebab-case)

**Examples:**
```
esphome/apollo-msr-2-173d74.yaml    # Device + MAC suffix
esphome/atom-s3-lite-0.yaml         # Device + number
esphome/plug-12.yaml                # Type + number
esphome/sonoff-s31-12.yaml          # Brand + model + number
```

### ESPHome Internal Naming

```yaml
esphome:
  name: plug12                   # Hostname: no underscores, stable
  friendly_name: "Bedroom Plug"  # Display name: changeable
```

**See detailed guide:** [Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)

---

## Multi-Area Entities

Entities that span multiple rooms or the entire home require special area designation.

### Whole-Home Entities

**Use `home` when:** Entity affects or represents the entire house.

**Examples:**
```
climate.home                    # Whole-home HVAC
alarm_control_panel.home        # Security system
weather.home                    # Home weather
sensor.home_average_temperature # Calculated across all rooms
sensor.home_total_power         # Whole-home power
```

### Floor/Level Entities

**Use floor names when:** Entity affects an entire floor.

**Examples:**
```
light.upstairs_all              # All upstairs lights
vacuum.upstairs                 # Upstairs robot vacuum
sensor.downstairs_temperature   # Downstairs zone sensor
```

### Exterior Entities

**Use `exterior` or specific outdoor area:**

**Examples:**
```
light.frontyard_sconces         # Front yard lights
camera.backyard_clear           # Backyard camera
sensor.exterior_temperature     # General outdoor temp
automation.exterior_sunset_lights_on # All exterior lights
```

**See detailed guide:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Temporary and Seasonal Entities

### Seasonal Decorations

**Pattern:** `<domain>.<area>_<season>_<device>`

**Examples:**
```
switch.frontyard_christmas_lights
switch.backyard_halloween_lights
light.porch_holiday_lights
automation.exterior_sunset_christmas_lights_on
```

### Temporary Devices

**Pattern:** `<domain>.<area>_temp_<device>` or `<domain>.<area>_guest_<device>`

**Examples:**
```
device_tracker.guest_phone_1    # Guest's phone
light.bedroom_guest_lamp        # Temporary guest lamp
switch.garage_temp_heater       # Temporary heater
```

**See detailed guide:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Disabled Entities

### Deprecated Entities

**Pattern:** Keep original name, use metadata to indicate status

**DO NOT modify entity ID:**
```yaml
# ❌ Bad
switch.old_bedroom_light
switch.deprecated_kitchen_fan

# ✅ Good
switch.bedroom_light
  enabled: false
  description: "Deprecated 2025-10-29: Replaced by light.bedroom_ceiling"
```

**Rationale:** Entity ID describes what it IS, not its current state.

**See detailed guide:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Integration-Specific Patterns

### ESPHome Entities

**Rename device-generated names to match location:**

```yaml
# Device-generated
sensor.apollo_msr_2_co2
sensor.apollo_msr_2_temperature

# Renamed to location
sensor.kitchen_co2
sensor.kitchen_temperature
```

### Z-Wave/Zigbee Entities

**Rename functional entities, keep diagnostic entities:**

```yaml
# Functional (rename)
light.2_1_dimmer → light.rec_room
sensor.z_wave_thermostat_temperature → sensor.living_temperature

# Diagnostic (keep)
sensor.node_6_rssi              # Keep node reference
sensor.node_6_last_seen         # Keep node reference
```

### Template Entities

**Use standard naming, indicate calculation in function:**

```yaml
sensor.indoor_average_humidity  # ✅ Calculated average
binary_sensor.hvac_fan_status   # ✅ Derived status
sensor.home_total_power         # ✅ Sum of all power
```

**DO NOT use `template_` prefix:**
```yaml
# ❌ Bad
sensor.template_average_humidity

# ✅ Good
sensor.indoor_average_humidity
```

**See detailed guide:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Virtual and Calculated Entities

### Template Sensors

**Pattern:** Use standard naming, indicate calculation in function token

**Examples:**
```
sensor.indoor_average_humidity  # Average across sensors
sensor.home_total_power         # Sum of power sensors
sensor.open_window_count        # Count of open windows
binary_sensor.anyone_home       # Presence aggregation
```

**Function Tokens for Calculations:**
- `average_<measurement>` - Mean value
- `total_<measurement>` - Sum
- `min_<measurement>` - Minimum
- `max_<measurement>` - Maximum
- `<measurement>_count` - Count
- `<measurement>_rate` - Rate of change

**See detailed guide:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Friendly Names

Friendly names are shown in the UI and should use natural language.

**Rules:**
- Use Title Case for proper nouns
- Use natural language with spaces
- Add qualifiers for duplicates in parentheses
- Can differ from entity ID

**Examples:**
```yaml
light.bedroom_ceiling:
  friendly_name: "Bedroom Ceiling Light"

light.bedroom_bedside:
  friendly_name: "Bedroom Lamp (Bedside)"

sensor.kitchen_temperature:
  friendly_name: "Kitchen Temperature"

automation.bedroom_motion_light_on:
  friendly_name: "Bedroom Motion → Light On"
```

---

## Devices

Device names can include brand, model, and location information.

**Pattern:** `<Brand> <Model>` or `<Brand> <Model> – <Location>`

**Examples:**
```
Hue Bulb                       # Brand + model
Hue Bulb – Office              # With location
Sonos Roam                     # Portable device
Apollo MSR-2                   # Sensor device
Zooz ZEN32 Scene Controller    # Controller device
```

**Device vs. Entity Naming:**
- Devices can have technical names
- Entities should be user-friendly
- Entities inherit from device but can be renamed

**See detailed guide:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Quick Reference Tables

### Entity Type Patterns

| Entity Type | Pattern | Example |
|-------------|---------|---------|
| light | `light.<area>_<location>` | [`light.bedroom_ceiling`](naming.md:1) |
| switch | `switch.<area>_<location>_<function>` | [`switch.kitchen_island_outlet`](naming.md:1) |
| sensor | `sensor.<area>_<location>_<measurement>` | [`sensor.bedroom_temperature`](naming.md:1) |
| binary_sensor | `binary_sensor.<area>_<location>_<detection>` | [`binary_sensor.hallway_motion`](naming.md:1) |
| media_player | `media_player.<area>_<device>` | [`media_player.living_room_tv`](naming.md:1) |
| camera | `camera.<area>_<location>` | [`camera.frontyard`](naming.md:1) |
| climate | `climate.<area>` | [`climate.home`](naming.md:1) |
| fan | `fan.<area>_<type>` | [`fan.bedroom_ceiling`](naming.md:1) |
| lock | `lock.<area>_door` | [`lock.front_door`](naming.md:1) |
| vacuum | `vacuum.<area>` | [`vacuum.upstairs`](naming.md:1) |
| automation | `automation.<area>_<trigger>_<action>` | [`automation.bedroom_motion_light_on`](naming.md:1) |
| script | `script.<area>_<action>` | [`script.bedroom_wake_routine`](naming.md:1) |
| scene | `scene.<area>_<mood>` | [`scene.living_room_movie`](naming.md:1) |

### Helper Entity Patterns

| Helper Type | Pattern | Example |
|-------------|---------|---------|
| input_boolean | `input_boolean.<mode>_mode` | [`input_boolean.guest_mode`](naming.md:1) |
| input_number | `input_number.<purpose>_<value>` | [`input_number.temperature_threshold`](naming.md:1) |
| input_select | `input_select.<purpose>_<options>` | [`input_select.house_mode`](naming.md:1) |
| input_text | `input_text.<purpose>_<content>` | [`input_text.alarm_message`](naming.md:1) |
| input_datetime | `input_datetime.<event>_<aspect>` | [`input_datetime.alarm_time`](naming.md:1) |
| timer | `timer.<activity>_timer` | [`timer.laundry_timer`](naming.md:1) |
| counter | `counter.<item>_count` | [`counter.doorbell_rings`](naming.md:1) |

---

## Common Location Tokens

### Furniture and Fixtures

| Token | Meaning | Example |
|-------|---------|---------|
| `ceiling` | Ceiling-mounted | [`light.bedroom_ceiling`](naming.md:1) |
| `wall` | Wall-mounted | [`switch.kitchen_wall`](naming.md:1) |
| `floor` | Floor-level | [`light.bedroom_floor`](naming.md:1) |
| `desk` | Desk/table | [`light.office_desk`](naming.md:1) |
| `bedside` | Beside bed | [`light.bedroom_bedside`](naming.md:1) |
| `nightstand` | Nightstand | [`light.bedroom_nightstand`](naming.md:1) |
| `table` | Table | [`light.living_room_table`](naming.md:1) |

### Architectural Elements

| Token | Meaning | Example |
|-------|---------|---------|
| `window` | Window | [`binary_sensor.bedroom_window`](naming.md:1) |
| `door` | Door | [`binary_sensor.front_door`](naming.md:1) |
| `chandelier` | Chandelier | [`light.entryway_chandelier`](naming.md:1) |
| `sconce` | Wall sconce | [`light.frontyard_sconces`](naming.md:1) |
| `pendant` | Pendant light | [`light.kitchen_pendant`](naming.md:1) |
| `recessed` | Recessed light | [`light.living_room_recessed`](naming.md:1) |

### Appliances and Features

| Token | Meaning | Example |
|-------|---------|---------|
| `island` | Kitchen island | [`switch.kitchen_island`](naming.md:1) |
| `countertop` | Countertop | [`switch.kitchen_countertop`](naming.md:1) |
| `under_cabinet` | Under cabinet | [`light.kitchen_under_cabinet`](naming.md:1) |
| `stove` | Stove/range | [`sensor.kitchen_stove`](naming.md:1) |
| `sink` | Sink | [`sensor.kitchen_sink`](naming.md:1) |
| `exhaust` | Exhaust fan | [`fan.bathroom_exhaust`](naming.md:1) |

### Directions and Positions

| Token | Meaning | Example |
|-------|---------|---------|
| `north` | North side | [`cover.living_room_north`](naming.md:1) |
| `south` | South side | [`cover.living_room_south`](naming.md:1) |
| `east` | East side | [`cover.bedroom_east`](naming.md:1) |
| `west` | West side | [`cover.bedroom_west`](naming.md:1) |
| `left` | Left side | [`light.bedroom_left`](naming.md:1) |
| `right` | Right side | [`light.bedroom_right`](naming.md:1) |
| `front` | Front | [`camera.front`](naming.md:1) |
| `back` | Back | [`camera.back`](naming.md:1) |

### Device Components

| Token | Meaning | Example |
|-------|---------|---------|
| `paddle` | Main paddle/rocker | [`select.entryway_paddle_single_tap`](naming.md:1) |
| `button1` | Button 1 | [`switch.entryway_button1_indication`](naming.md:1) |
| `button2` | Button 2 | [`switch.entryway_button2_indication`](naming.md:1) |
| `relay1` | Relay 1 | [`switch.garage_relay1`](naming.md:1) |
| `zone1` | Zone 1 | [`binary_sensor.kitchen_zone1_occupancy`](naming.md:1) |

---

## Common Function Tokens

### Measurements

| Token | Meaning | Unit | Example |
|-------|---------|------|---------|
| `temperature` | Temperature | °F/°C | [`sensor.bedroom_temperature`](naming.md:1) |
| `humidity` | Relative humidity | % | [`sensor.kitchen_humidity`](naming.md:1) |
| `pressure` | Barometric pressure | hPa/inHg | [`sensor.office_pressure`](naming.md:1) |
| `illuminance` | Light level | lux | [`sensor.hallway_illuminance`](naming.md:1) |
| `co2` | CO2 level | ppm | [`sensor.bedroom_co2`](naming.md:1) |
| `voc` | Volatile organic compounds | ppb | [`sensor.living_room_voc`](naming.md:1) |
| `pm25` | Particulate matter 2.5 | µg/m³ | [`sensor.bedroom_pm25`](naming.md:1) |

### Energy and Power

| Token | Meaning | Unit | Example |
|-------|---------|------|---------|
| `power` | Current power | W | [`sensor.kitchen_power`](naming.md:1) |
| `energy` | Energy consumption | kWh | [`sensor.home_energy`](naming.md:1) |
| `voltage` | Voltage | V | [`sensor.outlet_voltage`](naming.md:1) |
| `current` | Current | A | [`sensor.outlet_current`](naming.md:1) |

### Detection and Status

| Token | Meaning | Type | Example |
|-------|---------|------|---------|
| `motion` | Motion detected | binary | [`binary_sensor.hallway_motion`](naming.md:1) |
| `occupancy` | Area occupied | binary | [`binary_sensor.bedroom_occupancy`](naming.md:1) |
| `presence` | Person present | binary | [`binary_sensor.office_presence`](naming.md:1) |
| `contact` | Contact sensor | binary | [`binary_sensor.door_contact`](naming.md:1) |
| `leak` | Water leak | binary | [`binary_sensor.bathroom_leak`](naming.md:1) |
| `smoke` | Smoke detected | binary | [`binary_sensor.kitchen_smoke`](naming.md:1) |
| `battery` | Battery level | % | [`sensor.sensor_battery`](naming.md:1) |
| `signal` | Signal strength | dBm | [`sensor.device_signal`](naming.md:1) |

### Actions and Controls

| Token | Meaning | Example |
|-------|---------|---------|
| `restart` | Restart device | [`button.doorbell_restart`](naming.md:1) |
| `reboot` | Reboot device | [`button.router_reboot`](naming.md:1) |
| `identify` | Identify device | [`button.switch_identify`](naming.md:1) |
| `ping` | Ping device | [`button.node_ping`](naming.md:1) |
| `mode` | Mode selection | [`select.thermostat_mode`](naming.md:1) |
| `preset` | Preset selection | [`select.fan_preset`](naming.md:1) |
| `scene` | Scene selection | [`select.lighting_scene`](naming.md:1) |

### Calculations

| Token | Meaning | Example |
|-------|---------|---------|
| `average` | Mean value | [`sensor.indoor_average_temperature`](naming.md:1) |
| `total` | Sum | [`sensor.home_total_power`](naming.md:1) |
| `min` | Minimum | [`sensor.home_min_temperature`](naming.md:1) |
| `max` | Maximum | [`sensor.home_max_temperature`](naming.md:1) |
| `count` | Count | [`sensor.open_window_count`](naming.md:1) |
| `rate` | Rate of change | [`sensor.temperature_rate`](naming.md:1) |

---

## Pattern Summary

### Entity ID Patterns

```
# Basic pattern
<domain>.<area>_<optional_location>_<optional_function>

# Automation pattern
automation.<area>_<trigger>_<action>

# Script pattern
script.<area>_<action>_<qualifier>

# Scene pattern
scene.<area>_<mood_or_activity>

# Helper pattern
input_<type>.<purpose>_<descriptor>
```

### File Naming Patterns

```
# Dashboard files (snake_case)
dashboards/<purpose>_dashboard.yaml

# Blueprint files (snake_case)
blueprints/<type>/<source>/<name>.yaml

# Package files (snake_case)
packages/<scope>.yaml

# Template files (snake_case)
templates/<purpose>.yaml

# ESPHome files (kebab-case)
esphome/<device-type>-<id>.yaml

# Theme files (snake_case)
themes/<theme_name>.yaml
```

### Slug Patterns

```
# Dashboard slugs (kebab-case)
<purpose>-dashboard

# Examples
humidity-dashboard
security-overview
bedroom-controls
```

---

## Example: Scene Controller (Zooz ZEN32)

This comprehensive example demonstrates how the 4-rule system applies to a complex, real-world device with multiple entity types.

### Device Overview

**Device:** Zooz ZEN32 Scene Controller  
**Location:** Entryway  
**Function:** Controls chandelier + 4 scene buttons

### Load (Wired Chandelier)

**Rule 2: Physicality (Location Only)**

[`light.entryway_chandelier`](naming.md:1)
- **domain:** `light`
- **area:** `entryway`
- **location:** `chandelier` (The "Which" - distinguishes from other lights)
- **function:** *(none - the domain implies it's a light)*

**Rationale:** Multiple lights could exist in entryway, so location distinguishes this specific fixture.

### Controller Settings

The ZEN32 exposes many configuration entities for the paddle and buttons.

#### Paddle Configuration

**Rule 4: Combination (Both Location and Function)**

[`select.entryway_paddle_single_tap`](naming.md:1)
- **domain:** `select`
- **area:** `entryway`
- **location:** `paddle` (The "Which" component)
- **function:** `single_tap` (The "What" action being configured)

[`select.entryway_paddle_double_tap`](naming.md:1)
- **domain:** `select`
- **area:** `entryway`
- **location:** `paddle`
- **function:** `double_tap`

**Rationale:** Must specify which component (paddle vs. buttons) AND which action (single vs. double tap).

#### Button Configuration

**Rule 4: Combination (Both Location and Function)**

[`switch.entryway_button1_indication`](naming.md:1)
- **domain:** `switch`
- **area:** `entryway`
- **location:** `button1` (The "Which" button)
- **function:** `indication` (The "What" attribute - LED indication)

[`select.entryway_button1_led_color`](naming.md:1)
- **domain:** `select`
- **area:** `entryway`
- **location:** `button1`
- **function:** `led_color`

[`switch.entryway_button2_indication`](naming.md:1)
- **domain:** `switch`
- **area:** `entryway`
- **location:** `button2`
- **function:** `indication`

**Rationale:** Must specify which button (1-4) AND which attribute (indication, LED color, etc.).

#### Device-Wide Settings

**Rule 3: Purpose (Function Only)**

[`number.entryway_local_dimming_speed`](naming.md:1)
- **domain:** `number`
- **area:** `entryway`
- **location:** *(none - applies to whole device)*
- **function:** `local_dimming_speed` (The "What" setting)

**Rationale:** Setting applies to entire device, not specific component, so only function is needed.

#### Device Identity

**Rule 2: Physicality (Location Only)**

[`update.entryway_scene_controller`](naming.md:1)
- **domain:** `update`
- **area:** `entryway`
- **location:** `scene_controller` (The "Which" device)
- **function:** *(none - update domain implies firmware/software)*

[`button.entryway_scene_controller_identify`](naming.md:1)
- **domain:** `button`
- **area:** `entryway`
- **location:** `scene_controller`
- **function:** `identify` (The "What" action)

### Automations (Binding Gestures to Actions)

Automations connect the scene controller buttons to actions.

**Pattern:** `automation.<area>_<button>_<gesture>_<action>`

[`automation.entryway_btn1_pressed_scene_evening`](naming.md:1)
- **area:** `entryway` (where controller is located)
- **trigger:** `btn1_pressed` (button 1 single press)
- **action:** `scene_evening` (activates evening scene)

**Alternative (more concise):**
[`automation.entryway_btn1_press_set_home_evening`](naming.md:1)

**More Examples:**
```
automation.entryway_btn2_pressed_scene_tv
automation.entryway_btn3_pressed_scene_night
automation.entryway_btn4_pressed_scene_all_off
automation.entryway_btn12_double_scene_bright
automation.entryway_paddle_double_chandelier_30pct
```

### Gesture Tokens

**Recommended gesture tokens for button automations:**

| Gesture | Token | Example |
|---------|-------|---------|
| Single press | `pressed` | [`automation.entryway_btn1_pressed_scene_evening`](naming.md:1) |
| Double press | `double` | [`automation.entryway_btn12_double_scene_bright`](naming.md:1) |
| Triple press | `triple` | [`automation.bedroom_btn1_triple_scene_bright`](naming.md:1) |
| Quadruple press | `quad` | [`automation.living_btn1_quad_scene_party`](naming.md:1) |
| Quintuple press | `quint` | [`automation.office_btn1_quint_scene_focus`](naming.md:1) |
| Long press | `held` | [`automation.bedroom_btn1_held_lights_dim`](naming.md:1) |
| Release | `released` | [`automation.bedroom_btn1_released_lights_stop`](naming.md:1) |

### Complete Entity List

**Summary of all ZEN32 entities following the 4-rule system:**

```yaml
# Load
light.entryway_chandelier                    # Rule 2

# Paddle configuration
select.entryway_paddle_single_tap            # Rule 4
select.entryway_paddle_double_tap            # Rule 4
select.entryway_paddle_held                  # Rule 4
select.entryway_paddle_released              # Rule 4

# Button 1 configuration
switch.entryway_button1_indication           # Rule 4
select.entryway_button1_led_color            # Rule 4
number.entryway_button1_led_brightness       # Rule 4

# Button 2-4 configuration (similar pattern)
switch.entryway_button2_indication           # Rule 4
switch.entryway_button3_indication           # Rule 4
switch.entryway_button4_indication           # Rule 4

# Device-wide settings
number.entryway_local_dimming_speed          # Rule 3
number.entryway_remote_dimming_speed         # Rule 3
select.entryway_switch_type                  # Rule 3

# Device identity
update.entryway_scene_controller             # Rule 2
button.entryway_scene_controller_identify    # Rule 4

# Automations
automation.entryway_btn1_pressed_scene_evening
automation.entryway_btn2_pressed_scene_tv
automation.entryway_btn3_pressed_scene_night
automation.entryway_btn4_pressed_scene_all_off
automation.entryway_btn12_double_scene_bright
automation.entryway_paddle_double_chandelier_30pct
```

---

## Detailed Documentation

For comprehensive coverage of all patterns, examples, and edge cases, see the detailed reference documentation:

### Core References

- **[Automation Naming Reference](naming/reference/automations_detailed.md)** (1,826 lines)
  - Complete automation patterns with trigger-action structure
  - Analysis of existing automations with proposed improvements
  - Decision trees and comprehensive examples
  - Button/controller automation patterns
  - Multi-area automation handling

- **[Script Naming Reference](naming/reference/scripts_detailed.md)** (2,627 lines)
  - Complete script patterns using action-oriented approach
  - Analysis of existing scripts
  - Scripts vs. automations decision guidance
  - Reusable vs. one-off script patterns
  - Parameterized script naming

- **[Scene Naming Reference](naming/reference/scenes_detailed.md)** (2,483 lines)
  - Complete scene patterns with area and mood/activity structure
  - Analysis of existing scenes with proposed improvements
  - Mood-based, activity-based, and time-based patterns
  - Multi-area scene handling
  - Scene-automation integration

- **[Entity Types Reference](naming/reference/entity_types_detailed.md)** (884 lines)
  - Patterns for 35+ entity types
  - Integration-specific patterns (ESPHome, Z-Wave, Zigbee, MQTT)
  - Helper entity patterns
  - System entity patterns
  - Migration examples from real inventory

- **[Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)** (1,545 lines)
  - Dashboard naming (slug, title, icon, file)
  - Blueprint naming and organization
  - Package, template, and ESPHome config naming
  - File organization strategies
  - Version control considerations

- **[Edge Cases Reference](naming/reference/edge_cases_detailed.md)** (2,445 lines)
  - Multi-area entity patterns
  - Temporary and seasonal entity handling
  - Disabled entity documentation
  - Integration-specific edge cases
  - Virtual and calculated entity patterns
  - Migration and legacy entity handling
  - Conflict resolution strategies

### Quick Navigation

**By Topic:**
- Need automation patterns? → [Automation Reference](naming/reference/automations_detailed.md)
- Need script patterns? → [Script Reference](naming/reference/scripts_detailed.md)
- Need scene patterns? → [Scene Reference](naming/reference/scenes_detailed.md)
- Need entity type patterns? → [Entity Types Reference](naming/reference/entity_types_detailed.md)
- Need configuration object patterns? → [Configuration Objects Reference](naming/reference/configuration_objects_detailed.md)
- Need edge case guidance? → [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

**By Entity Type:**
- Lights, switches, sensors → [Entity Types Reference](naming/reference/entity_types_detailed.md)
- Media players, cameras → [Entity Types Reference](naming/reference/entity_types_detailed.md)
- Climate, fans, weather → [Entity Types Reference](naming/reference/entity_types_detailed.md)
- Locks, alarms, sirens → [Entity Types Reference](naming/reference/entity_types_detailed.md)
- Helpers (input_*) → [Entity Types Reference](naming/reference/entity_types_detailed.md)

**By Situation:**
- Multi-area entities → [Edge Cases Reference](naming/reference/edge_cases_detailed.md)
- Temporary/seasonal → [Edge Cases Reference](naming/reference/edge_cases_detailed.md)
- Disabled/deprecated → [Edge Cases Reference](naming/reference/edge_cases_detailed.md)
- ESPHome/Z-Wave/MQTT → [Edge Cases Reference](naming/reference/edge_cases_detailed.md)
- Virtual/calculated → [Edge Cases Reference](naming/reference/edge_cases_detailed.md)

---

## Best Practices

### General Guidelines

1. **Be Consistent:** Use the same patterns across similar entities
2. **Be Clear:** Names should be immediately understandable
3. **Be Concise:** Keep entity IDs under 50 characters when possible
4. **Be Specific:** Use descriptive tokens, avoid generic names
5. **Be Systematic:** Follow the 4-rule system deterministically

### Naming Checklist

Before finalizing an entity name, verify:

- [ ] Follows the 4-rule system
- [ ] Area matches Home Assistant area definitions
- [ ] Location token is a noun (the "Which")
- [ ] Function token is a verb/attribute (the "What")
- [ ] No brand/model names in entity ID
- [ ] Uses underscores to separate tokens
- [ ] All lowercase
- [ ] Under 50 characters (preferred)
- [ ] Unique within the system
- [ ] Consistent with similar entities

### Common Mistakes to Avoid

**❌ Don't:**
- Use brand names as primary identifier: `sensor.aqara_temperature`
- Include model numbers: `light.hue_bulb_gen3`
- Use spaces or hyphens: `sensor.bedroom-temperature`
- Mix casing: `sensor.BedroomTemperature`
- Be too generic: `sensor.sensor1`, `light.light`
- Be too verbose: `sensor.bedroom_temperature_sensor_reading`
- Include implementation details: `sensor.template_average_temp`
- Use ambiguous terms: `sensor.bedroom_temp1`

**✅ Do:**
- Use location-based names: `sensor.bedroom_temperature`
- Keep it simple: `light.bedroom_ceiling`
- Use underscores: `sensor.bedroom_temperature`
- Use lowercase: `sensor.bedroom_temperature`
- Be descriptive: `sensor.bedroom_temperature`
- Be concise: `sensor.bedroom_temp` (if needed)
- Focus on purpose: `sensor.indoor_average_temperature`
- Use clear terms: `sensor.bedroom_temperature_main`

---

## Migration Guide

### Renaming Existing Entities

When renaming entities to follow these conventions:

1. **Plan:** Document current names and proposed new names
2. **Map:** Create mapping of old → new entity IDs
3. **Find:** Use tools to find all references (automations, scripts, scenes, dashboards)
4. **Update:** Update all references to use new names
5. **Test:** Verify all automations and scripts still work
6. **Commit:** Commit changes to version control

### Tools Available

- **[`export_entities.py`](tools/export_entities.py:1)** - Export entity inventory
- **[`propose_ids.py`](tools/propose_ids.py:1)** - Generate compliant entity IDs
- **[`find_references.py`](tools/find_references.py:1)** - Find entity references
- **[`ws_rename_entities.py`](tools/ws_rename_entities.py:1)** - Rename entities via WebSocket

### Migration Playbook

See **[`tools/migration_playbook.md`](tools/migration_playbook.md:1)** for detailed migration procedures.

---

## Validation

### Automated Validation

Use the provided tools to validate entity naming:

```bash
# Export current entities
python tools/export_entities.py

# Generate proposed compliant names
python tools/propose_ids.py

# Find references before renaming
python tools/find_references.py <entity_id>

# Rename entities
python tools/ws_rename_entities.py
```

### Manual Validation

Check each entity against the 4-rule system:

1. **Rule 1:** Is domain + area unique? If yes, stop.
2. **Rule 2:** Do multiple entities differ by location? If yes, add location.
3. **Rule 3:** Do multiple entities differ by function? If yes, add function.
4. **Rule 4:** Need both? Add both in order: location then function.

---

## Appendix A: Complete Pattern Reference

### All Entity Patterns

```
# Core entities
light.<area>_<location>
switch.<area>_<location>_<function>
sensor.<area>_<location>_<measurement>
binary_sensor.<area>_<location>_<detection>

# Media & entertainment
media_player.<area>_<device>
camera.<area>_<location>_<view>
remote.<area>_<device>

# Climate & environment
climate.<area>_<location>
fan.<area>_<type>
weather.<location>

# Security & safety
lock.<area>_door
alarm_control_panel.<area>
siren.<area>_<location>

# Cleaning
vacuum.<area>
lawn_mower.<area>

# Configuration & control
button.<device>_<action>
select.<device>_<setting>
number.<device>_<value>
text.<device>_<purpose>

# System
update.<device>_<component>
event.<device>_<trigger>
conversation.<assistant>_<area>
image.<source>_<content>

# Helpers
input_boolean.<purpose>_<state>
input_number.<purpose>_<value>
input_select.<purpose>_<options>
input_text.<purpose>_<content>
input_datetime.<event>_<aspect>
timer.<activity>_timer
counter.<item>_count

# Location & presence
zone.<location>
person.<name>
device_tracker.<identifier>

# Automations, scripts, scenes
automation.<area>_<trigger>_<action>
script.<area>_<action>_<qualifier>
scene.<area>_<mood_or_activity>
```

---

## Appendix B: Area Definitions

### Standard Areas

**Indoor Areas:**
```
bedroom, master_bedroom, guest_bedroom
living_room, family_room, rec_room
kitchen, dining_room
bathroom, master_bathroom, guest_bathroom
office, study
hallway, entryway, foyer
closet, pantry, laundry_room
garage, basement, attic
```

**Outdoor Areas:**
```
frontyard, backyard
driveway, porch, deck, patio
garden, shed, garage_exterior
```

**Multi-Area Designations:**
```
home           # Whole home
upstairs       # Upper floor
downstairs     # Lower floor
exterior       # All outdoor
interior       # All indoor
```

### Area Assignment Rules

1. **Single Room:** Use specific room name
2. **Multiple Rooms:** Use primary room or combined name
3. **Entire Floor:** Use floor name (upstairs, downstairs)
4. **Whole Home:** Use `home`
5. **Exterior:** Use `exterior` or specific outdoor area
6. **No Physical Location:** Use `system` or omit area

---

## Appendix C: Token Guidelines

### Location Token Guidelines

**Use location tokens when:**
- Multiple entities of same type in area
- Physical location is distinguishing factor
- Location is meaningful and stable

**Location token should be:**
- A noun (the "Which")
- Physical place or component
- Stable (doesn't change frequently)
- Specific enough to distinguish

**Examples:**
```
✅ ceiling, bedside, window, door, island
❌ thing1, device, sensor, switch
```

### Function Token Guidelines

**Use function tokens when:**
- Multiple entities differ by purpose
- Measurement or action is distinguishing factor
- Function is more meaningful than location

**Function token should be:**
- A verb or attribute (the "What")
- Describes purpose or measurement
- Clear and specific
- Standard terminology when possible

**Examples:**
```
✅ temperature, motion, battery, restart
❌ thing, data, value, status1
```

### Multi-Word Tokens

**When tokens need multiple words:**
- Use underscores to connect: `under_cabinet`, `led_color`, `single_tap`
- Keep concise: prefer `temp` over `temperature` if length is issue
- Be consistent: use same terms across similar entities

**Examples:**
```
✅ under_cabinet, led_color, single_tap, automation_enabled
❌ undercabinet, ledcolor, singletap, automationenabled
```

---

## Appendix D: Special Situations

### When Area is Ambiguous

**Scenario:** Entity could belong to multiple areas.

**Solution:** Use the primary or most frequently associated area.

**Example:**
```
# Sensor in open-concept living/kitchen
sensor.living_room_temperature  # Use primary area
# OR
sensor.living_kitchen_temperature # Use combined area
```

### When Function is Complex

**Scenario:** Function requires multiple words.

**Solution:** Use underscores to connect words, keep as concise as possible.

**Examples:**
```
input_boolean.bedroom_shade_automation_enabled
input_number.bedroom_shade_night_position
sensor.bedroom_shade_reported_position
number.entryway_local_dimming_speed
```

### When Both Location and Function are Long

**Scenario:** Both location and function are multi-word.

**Solution:** Consider abbreviations or restructuring.

**Examples:**
```
# Long but acceptable
number.rec_room_light_dimming_speed_up_remote

# Consider abbreviating if too long
number.rec_room_light_dim_speed_up_remote

# Or restructure
number.rec_room_dimmer_speed_up_remote
```

---

## Appendix E: Integration-Specific Guidance

### ESPHome Devices

**Strategy:** Rename entities to match physical location.

**Before:**
```
sensor.apollo_msr_2_173d74_co2
sensor.apollo_msr_2_173d74_temperature
binary_sensor.apollo_msr_2_173d74_occupancy
```

**After:**
```
sensor.kitchen_co2
sensor.kitchen_temperature
binary_sensor.kitchen_occupancy
```

**See:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md) for complete ESPHome guidance.

### Z-Wave/Zigbee Devices

**Strategy:** Rename functional entities, keep diagnostic entities.

**Functional (rename):**
```
light.2_1_dimmer → light.rec_room
sensor.z_wave_thermostat_temperature → sensor.living_temperature
```

**Diagnostic (keep):**
```
sensor.node_6_rssi              # Keep node reference
sensor.node_6_last_seen         # Keep node reference
```

**See:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md) for complete Z-Wave guidance.

### MQTT Entities

**Strategy:** Configure with proper names from the start.

**Manual Configuration:**
```yaml
mqtt:
  sensor:
    - name: "Garage Temperature"
      unique_id: garage_temperature
      state_topic: "home/garage/temperature"
```

**See:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md) for complete MQTT guidance.

### Template Entities

**Strategy:** Use standard naming, indicate calculation in function.

**Examples:**
```
sensor.indoor_average_humidity  # ✅ Calculated average
binary_sensor.hvac_fan_status   # ✅ Derived status
sensor.home_total_power         # ✅ Sum of all power
```

**DO NOT use `template_` prefix:**
```
❌ sensor.template_average_humidity
✅ sensor.indoor_average_humidity
```

**See:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md) for complete template guidance.

---

## Appendix F: Troubleshooting

### Common Issues and Solutions

#### Issue: Entity ID Too Long

**Problem:** Entity ID exceeds 50 characters.

**Solutions:**
1. Abbreviate long words: `temperature` → `temp`, `humidity` → `humid`
2. Remove redundant words: `light.bedroom_ceiling_light` → `light.bedroom_ceiling`
3. Simplify location: `light.bedroom_left_hand_side` → `light.bedroom_left`
4. Restructure: `sensor.bedroom_window_contact_battery` → `sensor.bedroom_window_battery`

#### Issue: Duplicate Names

**Problem:** Two entities want the same name.

**Solutions:**
1. Add location token: `light.bedroom` → `light.bedroom_ceiling`, `light.bedroom_bedside`
2. Add function token: `sensor.kitchen` → `sensor.kitchen_temperature`, `sensor.kitchen_humidity`
3. Add qualifier: `sensor.bedroom_temperature` → `sensor.bedroom_temperature_main`, `sensor.bedroom_temperature_backup`

#### Issue: Unclear Which Rule Applies

**Problem:** Not sure which rule to use.

**Solution:** Work through the decision tree:
1. Is domain + area unique? → Rule 1
2. Differ by location? → Rule 2
3. Differ by function? → Rule 3
4. Need both? → Rule 4

#### Issue: Integration Creates Bad Names

**Problem:** Integration creates entities with poor names.

**Solutions:**
1. **ESPHome:** Configure entity names in ESPHome YAML
2. **Z-Wave/Zigbee:** Rename via UI after inclusion
3. **MQTT:** Use discovery with proper naming
4. **Templates:** Define with compliant IDs from start

**See:** [Edge Cases Reference](naming/reference/edge_cases_detailed.md) for integration-specific solutions.

---

## Appendix G: Version History

### Version 2.0 (2025-10-29)

**Major Update:** Comprehensive specification with detailed reference documentation.

**Changes:**
- Added Quick Start Guide for rapid onboarding
- Expanded entity type coverage from 9 to 35+ types
- Added comprehensive automation naming patterns
- Added comprehensive script naming patterns
- Added comprehensive scene naming patterns
- Added configuration object naming (dashboards, blueprints, etc.)
- Added edge case documentation
- Created detailed reference documentation (6 files, 11,000+ lines)
- Added decision trees and flowcharts
- Added quick reference tables
- Added migration guidance
- Added validation tools documentation

**Structure:**
- Main specification: ~3,500 lines (this file)
- Detailed references: 6 files, 11,000+ lines total
- Hybrid approach: Quick reference here, comprehensive details in references

### Version 1.0 (Original)

**Initial specification:**
- Core 4-rule system
- Basic entity ID patterns
- Light, switch, sensor, binary_sensor examples
- Scene controller example
- 184 lines

---

## Appendix H: Contributing

### Proposing Changes

To propose changes to this naming specification:

1. **Document:** Clearly describe the proposed change
2. **Justify:** Explain why the change is needed
3. **Examples:** Provide before/after examples
4. **Impact:** Assess impact on existing entities
5. **Review:** Submit for review and discussion

### Reporting Issues

If you find issues with the naming specification:

1. **Describe:** Clearly describe the issue
2. **Example:** Provide specific example
3. **Expected:** Describe expected behavior
4. **Actual:** Describe actual behavior
5. **Suggest:** Suggest potential solution

---

## Appendix I: Glossary

### Key Terms

**Area:** Physical room or zone in Home Assistant (e.g., bedroom, kitchen, exterior)

**Location Token:** The "Which" - Physical place or component (noun) that distinguishes entities by position

**Function Token:** The "What" - Purpose, measurement, or attribute (verb/attribute) that distinguishes entities by function

**Domain:** Entity type in Home Assistant (e.g., light, sensor, switch)

**Entity ID:** Unique identifier for an entity (e.g., `light.bedroom_ceiling`)

**Friendly Name:** Human-readable name shown in UI (e.g., "Bedroom Ceiling Light")

**Slug:** URL-friendly identifier (e.g., `humidity-dashboard`)

**Helper Entity:** Virtual entity for automation logic (input_boolean, input_number, etc.)

**Template Entity:** Virtual entity created through templates (calculated, derived, aggregated)

**Multi-Area Entity:** Entity affecting multiple rooms or entire home

**Virtual Entity:** Entity not representing physical device (template, group, aggregation)

### Naming Conventions

**snake_case:** Lowercase with underscores (e.g., `bedroom_ceiling`, `file_name.yaml`)

**kebab-case:** Lowercase with hyphens (e.g., `humidity-dashboard`, `device-name`)

**Title Case:** Capitalize main words (e.g., "Bedroom Ceiling Light", "Motion Activated")

**UPPERCASE:** All capitals (avoid in entity IDs)

**camelCase:** First word lowercase, subsequent words capitalized (avoid in entity IDs)

---

## Appendix J: FAQ

### Frequently Asked Questions

**Q: Should I rename all my existing entities?**

A: Not necessarily. Focus on:
1. New entities (follow conventions from start)
2. Problematic entities (confusing, inconsistent)
3. High-impact entities (frequently used in automations)
4. Gradual migration (rename as you touch entities)

**Q: What if my entity name is too long?**

A: Options:
1. Abbreviate long words (temperature → temp)
2. Remove redundant words
3. Simplify location/function tokens
4. Restructure the name
5. Accept longer names if clarity is important

**Q: Can I use hyphens in entity IDs?**

A: No. Entity IDs must use underscores only. Use hyphens only in:
- Dashboard slugs (kebab-case)
- ESPHome hostnames (kebab-case)
- File names (can use either, but prefer snake_case)

**Q: Should I include brand names in entity IDs?**

A: Generally no. Use location-based names instead:
- ❌ `sensor.aqara_temperature`
- ✅ `sensor.bedroom_temperature`

Exception: When brand is the distinguishing factor:
- ✅ `media_player.sonos_roam` (portable, brand is identifier)

**Q: How do I name entities from multi-sensor devices?**

A: Rename each sensor individually to match location + measurement:
```
sensor.apollo_msr_2_co2 → sensor.kitchen_co2
sensor.apollo_msr_2_temperature → sensor.kitchen_temperature
sensor.apollo_msr_2_humidity → sensor.kitchen_humidity
```

**Q: What about entities that move (mobile devices)?**

A: Don't use area tokens for mobile entities:
```
✅ device_tracker.ben_iphone
✅ device_tracker.nicole_iphone
❌ device_tracker.bedroom_ben_iphone
```

**Q: How do I name whole-home entities?**

A: Use `home` as the area:
```
✅ climate.home
✅ alarm_control_panel.home
✅ sensor.home_average_temperature
```

**Q: Should scenes include the word "scene"?**

A: No, the domain already indicates it's a scene:
```
❌ scene.bedroom_scene_bright
✅ scene.bedroom_bright
```

**Q: How do I name automations that activate scenes?**

A: Include "scene" in the action:
```
✅ automation.entryway_btn1_pressed_scene_evening
✅ automation.bedroom_bedtime_scene_dim
```

**Q: What if I have multiple temperature sensors in one room?**

A: Use location tokens to distinguish:
```
sensor.bedroom_window_temperature
sensor.bedroom_door_temperature
sensor.bedroom_ceiling_temperature
```

**Q: How do I name disabled/deprecated entities?**

A: Keep original name, use metadata:
```yaml
switch.bedroom_light
  enabled: false
  description: "Deprecated 2025-10-29: Replaced by light.bedroom_ceiling"
```

**Q: Can I use numbers in entity IDs?**

A: Yes, but prefer descriptive names:
```
⚠️ Acceptable: light.bedroom_1, light.bedroom_2
✅ Better: light.bedroom_ceiling, light.bedroom_bedside
```

**Q: How do I name template/virtual entities?**

A: Use standard naming, don't add `template_` or `virtual_` prefix:
```
❌ sensor.template_average_temperature
✅ sensor.indoor_average_temperature
```

**Q: What about seasonal entities like Christmas lights?**

A: Include season in name or use tags:
```
✅ switch.frontyard_christmas_lights
✅ switch.frontyard_lights (with tags: [seasonal, christmas])
```

**Q: How do I handle entities during migration?**

A: Run old and new in parallel, then deprecate old:
1. Create new entity with proper name
2. Run both for testing period
3. Update all references
4. Disable old entity
5. Delete after grace period

---

## Appendix K: Resources

### Documentation

- **Main Specification:** This file ([`naming.md`](naming.md:1))
- **Automation Reference:** [`naming/reference/automations_detailed.md`](naming/reference/automations_detailed.md:1)
- **Script Reference:** [`naming/reference/scripts_detailed.md`](naming/reference/scripts_detailed.md:1)
- **Scene Reference:** [`naming/reference/scenes_detailed.md`](naming/reference/scenes_detailed.md:1)
- **Entity Types Reference:** [`naming/reference/entity_types_detailed.md`](naming/reference/entity_types_detailed.md:1)
- **Configuration Objects Reference:** [`naming/reference/configuration_objects_detailed.md`](naming/reference/configuration_objects_detailed.md:1)
- **Edge Cases Reference:** [`naming/reference/edge_cases_detailed.md`](naming/reference/edge_cases_detailed.md:1)

### Tools

- **Export Entities:** [`tools/export_entities.py`](tools/export_entities.py:1)
- **Propose IDs:** [`tools/propose_ids.py`](tools/propose_ids.py:1)
- **Find References:** [`tools/find_references.py`](tools/find_references.py:1)
- **Rename Entities:** [`tools/ws_rename_entities.py`](tools/ws_rename_entities.py:1)
- **Migration Playbook:** [`tools/migration_playbook.md`](tools/migration_playbook.md:1)

### Naming Audit Project

This specification is the result of a comprehensive naming audit project:

- **Project Plan:** [`docs/naming_audit/00_project_plan.md`](docs/naming_audit/00_project_plan.md:1)
- **Tooling Phase:** [`docs/naming_audit/01_phase0_tooling.md`](docs/naming_audit/01_phase0_tooling.md:1)
- **Specification Phase:** [`docs/naming_audit/02_phase1_specification.md`](docs/naming_audit/02_phase1_specification.md:1)
- **Findings:** [`docs/naming_audit/findings/`](docs/naming_audit/findings/:1)
- **Decisions:** [`docs/naming_audit/decisions/`](docs/naming_audit/decisions/:1)

---

## Summary

This comprehensive naming guide provides:

1. **Quick Start:** 5-minute overview for rapid onboarding
2. **Core System:** The deterministic 4-rule naming system
3. **Entity Types:** Patterns for 35+ entity types
4. **Automations:** Comprehensive automation naming patterns
5. **Scripts:** Complete script naming patterns
6. **Scenes:** Detailed scene naming patterns
7. **Configuration:** Dashboard, blueprint, package, template, ESPHome naming
8. **Edge Cases:** Multi-area, temporary, disabled, integration-specific patterns
9. **References:** Links to detailed documentation (11,000+ lines)
10. **Tools:** Validation and migration tools
11. **Examples:** 200+ real-world examples
12. **Decision Trees:** Visual guides for naming decisions

**Key Principles:**
- **Deterministic:** Follow the 4-rule system
- **Consistent:** Use same patterns for similar entities
- **Clear:** Names should be immediately understandable
- **Maintainable:** Easy to find and update entities
- **Scalable:** Works for simple and complex setups

**Next Steps:**
1. Review the Quick Start Guide
2. Apply the 4-rule system to new entities
3. Consult detailed references for specific patterns
4. Use provided tools for validation and migration
5. Follow best practices for consistency

---

**Document Version:** 2.0  
**Last Updated:** 2025-10-29  
**Status:** Comprehensive Specification  
**Lines:** ~3,500  
**Coverage:** Complete naming system with detailed references

---

*End of Main Specification*

For detailed coverage of specific topics, see the reference documentation linked throughout this guide.
