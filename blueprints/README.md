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

## Setup Instructions

### Step 1: Create Required Helpers

Before using this blueprint, you must create the following helper entities in Home Assistant:

#### 1. Valve Opening Percentage (Sensor Group - Maximum)
- **Go to**: Settings > Devices & Services > Helpers > Create Helper > Group
- **Type**: Sensor
- **Name**: `% Valve Open` (or your preferred name)
- **Unit of Measurement**: `%`
- **Group Mode**: `Maximum`
- **Members**: Add all your Danfoss radiator valve opening sensors
- **Result Entity ID**: `sensor.valve_open` (or similar)

This helper tracks the highest valve opening across all radiators, which determines if the boiler is needed.

#### 2. Heat Required (Binary Sensor Group)
- **Go to**: Settings > Devices & Services > Helpers > Create Helper > Group
- **Type**: Binary Sensor
- **Name**: `Heat Required` (or your preferred name)
- **Group Mode**: `Any`
- **Members**: Add your boiler demand sensors from your heating system
- **Result Entity ID**: `binary_sensor.heat_required` (or similar)

This helper indicates if any radiator requires heat.

#### 3. Bathroom Radiator Timer (Timer)
- **Go to**: Settings > Devices & Services > Helpers > Create Helper > Timer
- **Name**: `Bathroom Radiator` (or your preferred name)
- **Duration**: Leave empty or set to `00:20:00` as default
- **Result Entity ID**: `timer.bathroom_radiator` (or similar)

This timer controls how long the bathroom radiator runs after humidity is detected. Duration is set by the blueprint automation.

### Step 2: Install the Blueprint

1. Copy the blueprint file to your Home Assistant `blueprints/automation/` directory
2. Restart Home Assistant or reload automations
3. In the UI, go to **Settings > Automations & Scenes > Blueprints**
4. Click **Create Automation** and select "Gas Boiler Control"

### Step 3: Configure the Automation

5. Select your entities from the helpers created in Step 1:
   - **Boiler demand sensor**: Select the "Heat Required" binary sensor group
   - **Radiator valve opening sensor**: Select the "% Valve Open" sensor group
   - **Bathroom radiator timer**: Select the "Bathroom Radiator" timer
6. Configure remaining inputs:
   - Select your bathroom humidity sensor
   - Select your outdoor temperature sensor
   - Select your boiler/bathroom radiator switch
   - Adjust thresholds as needed for your climate
7. Save and enable the automation

## Customization

All thresholds and timings are configurable through the UI. Adjust based on:
- Your heating system's response time
- Local climate and seasonal variations
- Comfort preferences
- Energy efficiency goals

### Recommended Starting Values
- **Valve opening threshold**: 29% (default)
- **High humidity threshold**: 65%
- **Optimal humidity threshold**: 30%
- **Timer duration**: 1200 seconds (20 minutes)
- **Outdoor temperature threshold**: 18°C

## Requirements

- Home Assistant 2021.1.0 or newer
- At least one Danfoss radiator valve (or compatible valve with opening percentage sensor)
- Bathroom humidity sensor with % unit
- Outdoor temperature sensor
- Smart switch or relay for boiler control
- The three helper entities described above

## Troubleshooting

**Boiler not turning on?**
- Verify the valve opening percentage sensor is reporting correct values
- Check that the "Heat Required" binary sensor is updating correctly
- Ensure the valve opening threshold is set appropriately for your system

**Bathroom radiator not responding to humidity?**
- Check that the humidity sensor is configured correctly
- Verify humidity thresholds match your comfort preferences
- Ensure the bathroom radiator timer exists and is not locked

**Timer not starting?**
- Verify the timer helper is created and accessible
- Check that outdoor temperature is below the configured threshold
- Review Home Assistant logs for any automation errors

## Collaboration

These blueprints are maintained for collaboration with Danfoss and other heating system partners. For issues or improvements, please open an issue or discussion in this repository.
