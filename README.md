# Smart Battery Management Blueprint for Home Assistant

This blueprint provides a comprehensive battery management system that integrates with the Stenite Battery Planner and Ferroamp battery systems. It combines planning, control, and self-consumption features into a single, easy-to-configure automation.

## Features

- Automated battery planning based on electricity prices and consumption patterns
- Intelligent battery control (charging, discharging, idle)
- Self-consumption optimization
- Configurable update intervals and thresholds
- Integration with Stenite Battery Planner and Ferroamp systems

## Prerequisites

- Home Assistant
- [Stenite Battery Planner](https://github.com/Mewongu/hacs_stenite_battery_planner) integration
- Ferroamp integration
- Required sensors:
  - Battery state of charge sensor
  - Grid power import sensor
  - Grid power export sensor

## Installation



1.Either:  
[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FMewongu%2Fhass_blueprint_ferroamp_battery_optimization%2Fblob%2Fmain%2Fblueprint.yaml)  
Or copy the blueprint YAML file to your Home Assistant configuration folder:
   ```bash
   wget -O config/blueprints/automation/smart_battery_management.yaml https://raw.githubusercontent.com/your-repo/smart_battery_management.yaml
   ```

2. Create a helper entity for tracking discharge power:
   ```yaml
   input_number:
     last_discharge_power:
       name: Last Discharge Power
       min: -100000
       max: 100000
       step: 1
   ```

3. Create an automation using the blueprint through the Home Assistant UI:
   - Go to Configuration → Automations & Scenes
   - Click "+ Add Automation"
   - Click "Create new automation from blueprint"
   - Select "Smart Battery Management"
   - Configure the inputs as described below

## Configuration

### Required Inputs

| Input | Description | Example |
|-------|-------------|---------|
| `battery_soc_sensor` | Entity ID of the battery state of charge sensor | `sensor.ferroamp_system_state_of_charge` |
| `power_in_sensor` | Entity ID of the grid import power sensor | `sensor.active_power_in` |
| `power_out_sensor` | Entity ID of the grid export power sensor | `sensor.active_power_out` |
| `last_discharge_helper` | Entity ID of the helper for tracking discharge power | `input_number.last_discharge_power` |

### Optional Inputs

| Input | Description | Default | Range |
|-------|-------------|---------|--------|
| `planner_update_interval` | How often to update the battery plan (minutes) | 1 | 1-60 |
| `power_threshold` | Minimum power change threshold (watts) | 100 | 0-1000 |

## Example Configuration

```yaml
automation:
  - alias: "Smart Battery Management"
    use_blueprint:
      path: smart_battery_management.yaml
      input:
        battery_soc_sensor: sensor.ferroamp_system_state_of_charge
        power_in_sensor: sensor.active_power_in
        power_out_sensor: sensor.active_power_out
        last_discharge_helper: input_number.last_discharge_power
        planner_update_interval: 1
        power_threshold: 100
```

## Operating Modes

The blueprint handles four different operating modes:

1. **Charge Mode**: Battery charges at the recommended power level
2. **Discharge Mode**: Battery discharges at the recommended power level
3. **Idle Mode**: Battery neither charges nor discharges
4. **Self-Consumption Mode**: Battery dynamically adjusts to optimize self-consumption

The mode selection is handled automatically by the Stenite Battery Planner based on electricity prices and consumption patterns.

## Self-Consumption Logic

When in self-consumption mode, the blueprint:
1. Monitors grid import/export power
2. Calculates required battery power to minimize grid interaction
3. Applies hysteresis through the power threshold setting
4. Updates battery discharge power accordingly

## Support

- The blueprint itself: [Create an issue](https://github.com/Mewongu/hass_blueprint_ferroamp_battery_optimization/issues)
- Stenite Battery Planner: [Stenite issues](https://github.com/Mewongu/hacs_stenite_battery_planner/issues)

## Contributing

Pleas do. 

## License

This blueprint is licensed under the MIT License. See the LICENSE file for details.