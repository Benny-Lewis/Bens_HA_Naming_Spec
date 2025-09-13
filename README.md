# Ben's Home Assistant Naming Spec

*Last updated: 2025‑09‑12*

# Introduction

## Name Types at a Glance

### Entities, Devices & Network 

| Type | Format | Purpose | Example |
| --- | --- | --- | --- |
| **Entity ID** | `<domain>.<class>_<index>` (lowercase, underscores; index 3-digit zero‑padded) | Automations/code; stable machine key | `sensor.temp_107` |
| **Entity Friendly Name** | `[Area] [Thing]` (space) | UI/voice label for **entities**; each entity must have a unique friendly name | `Kitchen Temperature` |
| **Device Name** | **Required:** `Brand Model #<index>` | Physical hardware label surfaced by the integration | `SwitchBot Meter #107` |
| **Hostname** | `<host_prefix>-<index>` (hyphenated, lowercase) | Network identifier for the physical device | `plug-012` |

## Philosophy

Use stable, machine‑readable IDs and separate human‑friendly labels. Avoid embedding location, brand, or purpose in IDs; keep them short and reuse indices per device.

## Onboarding Example

1. Unbox a SwitchBot Meter for the Living Room.
2. Identify the correct index range for the device type: 100–199	Environmental sensors
3. Pick the next free index in that range, e.g., 107.
4. Create entity IDs `sensor.temp_107` and `sensor.humidity_107`.
5. Name the device `SwitchBot Meter #107`.
6. Set entity friendly names `Living Room Temperature` and `Living Room Humidity`.
7. Assign the entities to the `Living Room` area.
 
## The Core Naming Schema

### The Entity ID: The Machine Layer

#### Core Formula

```
entity_id = <domain>.<class>_<index>
```

* `<domain>` := integration-defined Home Assistant domain indicating the native device type (sensor, light, etc.) (see [home assistant domains](https://www.home-assistant.io/docs/configuration/entities_domains/#domains))
* `<class>` := generic functional type naming what the entity measures or actuates; prefer HA `device_class` terms
* `<index>` := zero-padded 3-digit slot number (001–999) reused across related entities from the same device

Detailed tables for `<domain>`, `<class>`, and `<index>` live in the [Reference Tables](#reference-tables) section.

##### Core ID Rules

**Do**

* Keep IDs short, generic, and stable.
* Lowercase with underscores; zero‑pad indices.
* Reuse an index across entities from the same device slot.
* Keep indices within the reserved ranges above.
* Use native domains (`humidifier`, `valve`, `climate`, etc.) instead of `switch` when available.

**Don't**

* Put location, purpose, brand, or person into the ID.
* Encode seasonal roles (e.g., `christmas_tree`) in the ID.

###### Anti‑patterns

* `sensor.kitchen_freezer_temperature` ← encodes location in ID
* `binary_sensor.mailbox_motion_driveway` ← multiple roles/places
* `switch.christmas_tree` ← seasonal purpose in ID
* `sensor.temp_fence_by_gate` ← location in ID; brittle when moved

#### Example

**Multi‑entity device** — SwitchBot Temp/RH on the back fence

Each entity requires its own friendly name following the `[Area] [Thing]` format.

| Property       | Value                                              |
| -------------- | -------------------------------------------------- |
| Entity IDs     | `sensor.temp_107`, `sensor.humidity_107`           |
| Friendly Names | `Backyard Fence Temperature`, `Backyard Fence Humidity` |
| Area           | Backyard                                           |
| Integration    | SwitchBot                                          |
| Notes          | Mounted on north fence post, AA x2                 |

**Plug→Light remap** — If a smart plug powers a lamp, create a `light:` wrapper so voice/UX behaves like a light.

### Reference Tables

#### `<domain>` Decoder (prefer native domains over `switch`)

| `<domain>`      | Use for                                           | Example                     |
| --------------- | ------------------------------------------------- | --------------------------- |
| `sensor`        | Measurements (temperature, humidity, co2, etc.)   | `sensor.temp_107`           |
| `binary_sensor` | True/false state (contact, motion, leak, etc.)    | `binary_sensor.contact_202` |
| `light`         | Native light entities (bulb, strip, sconce, etc.) | `light.bulb_504`            |
| `fan`           | Fan entities                                      | `fan.ceiling_601`           |
| `cover`         | Shades, blinds, garage/gate, awnings              | `cover.shade_702`           |
| `climate`       | Thermostats / HVAC controllers                    | `climate.thermostat_652`    |
| `humidifier`    | Humidifiers (native domain)                       | `humidifier.evaporative_653` |
| `valve`         | Solenoid/manual valves (native domain)            | `valve.zone_654`            |
| `lock`          | Door/bolt locks                                   | `lock.deadbolt_751`         |
| `camera`        | Cameras / doorbells                               | `camera.doorbell_801`       |
| `media_player`  | TVs, speakers, AVRs                               | `media_player.tv_821`       |
| `button`        | Stateless push actions (services)                 | `button.reset_861`          |
| `number`        | Numeric knobs/limits                              | `number.setpoint_862`       |
| `select`        | Enumerated choices                                | `select.mode_863`           |
| `siren`         | Sirens / alarms                                   | `siren.siren_655`           |
| `vacuum`        | Robot vacuums                                     | `vacuum.robot_656`          |
| `water_heater`  | Water heaters / boilers                           | `water_heater.tank_657`     |
| `remote`        | IR/RF transmitters/bridges                        | `remote.tx_822`             |
| `switch`        | Generic on/off loads **without** a native domain  | `switch.relay_612`          |

#### `<class>` Mapping

* This table maps your `<class>` token to the official Home Assistant `device_class` used by the UI, dashboards, and some integrations.
* Use these names when setting `device_class` to keep dashboards/search consistent.

| Token         | device_class    |
| ------------- | --------------- |
| `battery`     | `battery`       |
| `co2`         | `carbon_dioxide` |
| `contact`     | `opening`       |
| `humidity`    | `humidity`      |
| `illuminance` | `illuminance`   |
| `moisture`    | `moisture`      |
| `motion`      | `motion`        |
| `pm25`        | `pm25`          |
| `power`       | `power`         |
| `pressure`    | `pressure`      |
| `temp`        | `temperature`   |

##### Suggested `<class>` tokens for actuators

Use these examples to keep actuator names consistent. Pick the token that best fits your device.

| Domain | Suggested `<class>` tokens |
| ------ | -------------------------- |
| `light` | `bulb`, `dimmer`, `strip`, `sconce`, `lamp` |
| `switch` | `relay`, `outlet`, `plug`, `contactor` |
| `cover` | `shade`, `blind`, `gate`, `garage`, `awning` |
| `fan` | `ceiling`, `exhaust`, `circulator` |
| `lock` | `deadbolt`, `latch`, `strike` |

#### `<index>` Scheme (000–999)

Use a 3-digit, zero-padded **slot index** shared by all entities from the same physical device. Keep it location-agnostic and never reuse numbers.

##### Ranges

| Range      | Primary device role (choose one per **device**)                                   |
|------------|------------------------------------------------------------------------------------|
| **000–049**| Infrastructure/bridges & device-health (only if that’s the device’s primary role) |
| **050–099**| Reserved for future use |
| **100–199**| Environmental sensors (temp, humidity, pressure, CO₂/PM/VOC, illuminance, noise)  |
| **200–299**| Perimeter/entry sensors (contact, tilt, vibration for doors/windows/mailbox)      |
| **300–399**| Presence sensors (PIR, mmWave presence/occupancy, people counters)                 |
| **400–449**| Water & weather (leak/moisture, level/flow, rain/freeze/wind)                      |
| **450–499**| Safety sensors (smoke/heat/CO, glass-break)                                        |
| **500–599**| Lighting actuators (bulbs, dimmers, light wrappers/groups)                         |
| **600–649**| Power switching & energy (plugs, relays, in-wall switches; CT/energy meters)       |
| **650–699**| Appliances & utilities (water_heater, EVSE, major fixed loads)                     |
| **700–749**| Covers & access actuators (shades/blinds/garage/gate)                              |
| **750–799**| Locks & access control (lock domain, keypads)                                      |
| **800–819**| Cameras & doorbells                                                                |
| **820–839**| Media & remotes (media_player, IR/RF `remote`)                                     |
| **840–859**| Reserved for future physical categories                                            |
| **860–959**| **Software-defined** (virtual/derived/statistics/template/utility_meter). Require suffix: `*_avg`, `*_rate`, `*_sum`, `*_max`, `*_min`, `*_daily`, `*_calc`, `*_total`. |
| **960–979**| Test/Staging devices                                                                |
| **980–989**| Commissioning/temporary placeholders                                                |
| **990–999**| Retired/reserved (do not reuse)                                                    |

##### Assignment Rules
1. Pick the block by the **device’s primary role**; reuse that index for all its entities (e.g., `sensor.temp_207`, `sensor.humidity_207`, `sensor.battery_207`).
   For a multi-function device like a fan with a light, choose one primary role for the index range (e.g., the 500–599 lighting range) and use that same index for all entities from that device (e.g., `light.ceiling_501` and `fan.ceiling_501`).
2. Within a block, assign the **lowest free number**.
3. **Never repurpose** an index; mark retired hardware in `inventory.yaml`.
4. Put software-only/derived entities in **860–959** and use a derived suffix.

### The Human & Network Layer

* **Friendly Names** — `[Area] [Thing]`. Make them specific and spoken‑friendly. You *can* include **location and purpose**. Change them anytime—automations don’t break. If two identical Things exist in one Area, add a short qualifier in Friendly Name only: `Living Room Lamp (North)` / `Living Room Lamp (South)`. Good examples: **Freezer Temperature**, **Backyard Motion**, **Living Room Lamp**, **Nursery Air Quality (PM2.5)**.

* **Device Names** — `Brand Model #<index>`. Appears in integration/device views; label the physical hardware. Example: `SwitchBot Meter #107`.

* **Hostnames** — Use hyphens only. Allowed: `a–z`, `0–9`, and `-`; start/end with alphanumeric; max 63 chars. Pattern: `<host_prefix>-<index>` where `<index>` is the three-digit slot index (zero‑padded) and `host_prefix` is the primary class or device family (`plug`, `meter`, `doorbell`, `sonoff-s31`); keep it short (≤12 chars). Examples: `sonoff-s31-012`, `plug-012`, `meter-107`. Hostname is independent from Entity IDs and Friendly Names.

#### Spaces & Presence

| Type      | Purpose                  | Format in this spec     | Example   |
| --------- | ------------------------ | ----------------------- | --------- |
| **Area**  | Room/space grouping      | Plain words, Title Case | `Kitchen` |
| **Floor** | Building level grouping  | Plain words, Title Case | `Upper`   |
| **Zone**  | Geofence/presence places | Speakable place names   | `Daycare` |

* **Areas**: physical rooms/spaces for UI grouping, presence, and dashboards. Use short, human names (`Living Room`, `Garage`, `Back Yard`).
* **Floors**: `Basement`, `Lower`, `Main`, `Upper`, `Attic` (or `First`, `Second`). Don’t encode floor in IDs or Area names.
* **Zones** (map presence): natural place names you’d say aloud (`Home`, `Work`, `Daycare`, `Gym`). When needed for disambiguation, add a concise qualifier (`Work (Ben)`, `Work (Nicole)`).

## Control & Logic Objects

### Automations

```
Title: [Scope] When [Trigger], [Action] (Constraint)
ID (YAML only): auto_<scope>_<trigger>_<action>
```

Each `<scope>`, `<trigger>`, and `<action>` slug must be lowercase, alphanumeric, and use underscores for separators (lowercase with underscores).

Example: `Kitchen When Motion, Turn On Lamp (After Sunset)` → `auto_kitchen_motion_lamp_on`

Home Assistant's UI automatically generates unique, stable IDs (for example, `automation.16755...`) for automations created in the UI. In those cases, focus on a clear Title; a manual slug-style ID is optional and primarily used for automations defined in YAML.

### Scripts

```
Name: Do [Action] ([Scope])
ID: script.<slug>
```

The `<slug>` should be lowercase, alphanumeric, and use underscores for separators (lowercase with underscores).

Example: `Do Goodnight (Whole House)` → `script.goodnight`

### Scenes

```
Name: [Area] [Mood/State]
ID: scene.<slug>
```

The `<slug>` should be lowercase, alphanumeric, and use underscores for separators (lowercase with underscores).

Example: `Living Room Movie` → `scene.living_room_movie`

### Helpers (inputs, timers, counters, schedules)

```
Name: [Scope] [Thing]
ID: <prefix>.<slug>   (input_boolean | input_number | input_select | timer | counter | schedule)
```

The `<slug>` must be lowercase, alphanumeric, and use underscores for separators (lowercase with underscores).

Examples: `House Quiet Hours` → `input_boolean.quiet_hours`; `Sprinklers Runtime (min)` → `input_number.sprinklers_runtime`

### Groups

```
Name: [Area] All [ThingPlural]   or   All [ThingPlural]
ID: group.<slug>
```

Examples: `Kitchen All Lights` → `group.kitchen_lights`; `All Motion Sensors` → `group.all_motion_sensors`

## System Governance

### Governance (Adding/Changing Class Tokens)

Before adding a new `<class>` token, it must pass **all three**:

1. A native **domain** or `device_class` doesn’t already cover it.
2. The token is **generic** (no brand, purpose, or location leakage).
3. Two knowledgeable users would independently pick the **same** token for the same thing.

Changes should be proposed alongside updates to the **device-class mapping** (if relevant) and, if accepted, reflected in this decoder.

### inventory.yaml & Lookup Tools

A tiny catalog to anchor the slot/index → real hardware mapping. This complements slot-based indexing and accelerates audits/renames.

```yaml
# inventory.yaml
107:
  area: Backyard
  device: SwitchBot Meter #107
  installed: 2025-08-10
  notes: north fence post, AA x2
808:
  area: Front Door
  device: Eufy Cam 2C #808
```

Automations search/validation scripts can assert that each in-use index appears exactly once across entities and inventory.

Future utilities can leverage the schema above to provide reverse lookups, index audits, and bulk rename assistance. To make assigning new indices and finding devices easier, you can build simple tools directly within Home Assistant:

* **Inventory Viewer**: Create a dashboard view using a Markdown Card that displays the contents of your `inventory.yaml`. This gives you a quick, read-only reference of which index corresponds to which physical device without needing to open the file editor.
* **"Next Available Index" Sensor**: Create a Template Sensor that finds the next free index within a specific range. This helps you avoid manually scanning your `inventory.yaml` file. For example, a sensor could tell you "The next free index in the Lighting (500-599) range is 503."
* **Device Health Dashboard**: Use your consistent naming to your advantage. For example, you can create a dashboard that automatically lists all entities with a battery class (`sensor.battery_*`) that are below a certain threshold, immediately telling you which devices need new batteries. The Device Name (`Brand Model #<index>`) makes identifying the physical device trivial.

### Linter Rules

```yaml
zero_pad_indices: true

# Disallow obvious locations in IDs (case‑insensitive) — tune as needed.
forbid_location_regex: |
  (?i)(kitchen|kitch|pantry|dining|living(_)?room|lr|family|
  bed(room)?|br|nursery|hall|garage|office|study|den|loft|
  basement|attic|shed|porch|patio|deck|yard|back(_)?yard|
  front(_)?yard|garden|drive(way)?|stair|bath(room)?|toilet|wc|
  laundry|mud(room)?)

# Indices 860–959 reserved for virtual/derived; flag violations (advisory).
virtual_derived_index_range: [860, 959]
require_derived_class_suffix: true  # e.g., *_avg, *_daily, *_rate
```

