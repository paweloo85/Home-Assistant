# Home Assistant Blueprints

This directory contains reusable automation blueprints for Home Assistant, designed to work with various smart home components including Danfoss heating systems.

## Available Blueprints

### Gas Boiler Control (`automation/gas_boiler_blueprint.yaml`)

A comprehensive automation blueprint that intelligently controls a gas boiler based on multiple conditions:

#### Features
- **Radiator Demand Control**: Monitors radiator valve opening and boiler demand sensors to efficiently heat the system
- **Bathroom Humidity Management**: Automatically activates the bathroom radiator when humidity exceeds threshold, with automatic timer-based dehumidification
- **Temperature-Aware Logic**: Prevents unnecessary heating when outdoor temperature is warm enough
- **Periodic Monitoring**: Checks system state every 10 minutes to optimize boiler operation

#### Key Inputs

| Input | Type | Default | Purpose |
|-------|------|---------|---------|
| Boiler demand sensor | Binary Sensor | — | Indicates radiators need heat |
| Radiator valve opening sensor | Sensor (%) | — | Reports valve opening percentage |
| Valve opening threshold | Number | 29% | Minimum valve opening to trigger boiler |
| Boiler/bathroom radiator switch | Switch | — | Controls heating device |
| Bathroom radiator timer | Timer | — | Controls humidity-based heating duration |
| Humidity sensor | Sensor (%) | — | Bathroom humidity measurement |
| High humidity threshold | Number | 65% | Triggers bathroom heating |
| Optimal humidity threshold | Number | 30% | Stops bathroom heating |
| Timer duration | Number | 1200s | How long to run bathroom radiator |
| Outdoor temperature sensor | Sensor (°C) | — | Outdoor temperature measurement |
| Outdoor temperature threshold | Number | 18°C | Above this, bathroom radiator disabled |

#### Trigger Conditions

1. **Every 10 minutes** - Periodic state check
2. **Radiators turn boiler on** - When radiator demand goes high
3. **Radiators turn boiler off** - When radiator demand goes low
4. **Humidity too high** - When bathroom humidity exceeds threshold
5. **Humidity optimal** - When bathroom humidity drops to comfortable level
6. **Bathroom radiator timer finished** - When humidity-based heating timer expires

#### Supported Hardware

- **Danfoss** radiator valves and controls
- Any Home Assistant-compatible:
  - Binary sensors for boiler demand
  - Temperature and humidity sensors
  - Smart switches for boiler/radiator control
  - Timer entities

## Usage

1. Copy the blueprint file to your Home Assistant `blueprints/automation/` directory
2. Restart Home Assistant or reload automations
3. In the UI, go to **Settings > Automations & Scenes > Blueprints**
4. Click **Create Automation** and select "Gas Boiler Control"
5. Select your entities and adjust thresholds as needed
6. Save and enable the automation

## Customization

All thresholds and timings are configurable through the UI. Adjust based on:
- Your heating system's response time
- Local climate and seasonal variations
- Comfort preferences
- Energy efficiency goals

## Requirements

- Home Assistant 2021.1.0 or newer
- Compatible sensor entities (temperature, humidity, valve opening)
- A controllable switch or relay for the boiler

## Collaboration

These blueprints are maintained for collaboration with Danfoss and other heating system partners. For issues or improvements, please open an issue or discussion in this repository.
