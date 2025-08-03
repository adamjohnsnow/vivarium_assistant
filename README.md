# Vivarium Assistant - Complete ESP32-S3 Environmental Control System

A comprehensive environmental monitoring and control system for vivariums featuring dual ESP32-S3 displays, sensor monitoring, IR lighting control, and Home Assistant integration.

## 🔧 Hardware Components

### Primary Display Unit (ESP32-S3 AMOLED)

- **ESP32-S3 DevKit-C-1** - Main microcontroller with WiFi
- **RM67162 AMOLED Display** - 240x400px color touchscreen display
- **LTR390 UV/Light Sensor** - I2C UV index and ambient light sensor
- **HC-SR04 Ultrasonic Sensor** - Distance measurement for water level monitoring

### Secondary Display Unit (ESP32-S3 LCD)

- **ESP32-S3 DevKit-C-1** - Secondary controller
- **ST7789V LCD Display** - 240x320px color display with animations
- **Home Assistant Integration** - Real-time sensor data display

### Sensor Unit (ESP32)

- **ESP32 DevKit** - Dedicated sensor controller
- **SHT31 Temperature/Humidity Sensor** - I2C environmental sensor
- **2x DS18B20 Temperature Sensors** - OneWire waterproof probes for substrate and basking areas
- **10kΩ Resistor** - Pull-up resistor for OneWire bus

### Smart Lighting Control

- **Broadlink RM Mini 3** - IR blaster for TerraSky lighting control
- **TerraSky LED System** - Full spectrum reptile lighting with remote control
- **State Tracking** - Home Assistant input_select for lighting status monitoring

### Environmental Controls

- **Smart Fan** - WiFi controlled ventilation
- **Smart Mister** - WiFi controlled misting system  
- **Smart Fogger** - WiFi controlled fogging system

### Power Requirements

- **Input:** 5V USB or external power supply
- **Operating:** 3.3V (regulated on ESP32-S3 board)
- **Current:** ~200-500mA (depending on display brightness and sensors)

## 📡 Pin Connections

### ESP32-S3 AMOLED Unit (Primary Display)

| Component                       | ESP32-S3 Pin  | Function          | Notes                   |
| ------------------------------- | ------------- | ----------------- | ----------------------- |
| **AMOLED Display (RM67162)**    |               |                   |                         |
| CS                              | GPIO6         | Chip Select       | QSPI                    |
| Reset                           | GPIO17        | Hardware Reset    | Active Low              |
| Enable                          | GPIO38        | Display Enable    | Backlight Control       |
| CLK                             | GPIO47        | SPI Clock         | Quad SPI                |
| Data[0]                         | GPIO18        | SPI Data 0        | QSPI                    |
| Data[1]                         | GPIO7         | SPI Data 1        | QSPI                    |
| Data[2]                         | GPIO48        | SPI Data 2        | QSPI                    |
| Data[3]                         | GPIO5         | SPI Data 3        | QSPI                    |
| **LTR390 UV/Light Sensor**      |               |                   |                         |
| SDA                             | GPIO13        | I2C Data          | Pull-up integrated      |
| SCL                             | GPIO12        | I2C Clock         | Pull-up integrated      |
| VCC                             | 3.3V          | Power             | Critical - check connection |
| GND                             | GND           | Ground            |                         |
| **HC-SR04 Distance Sensor**     |               |                   |                         |
| Trigger                         | GPIO10        | Ultrasonic Trigger| 10µs pulse              |
| Echo                            | GPIO11        | Echo Response     | Distance measurement    |
| VCC                             | 5V            | Power             | Requires 5V             |
| GND                             | GND           | Ground            |                         |

### ESP32-S3 LCD Unit (Secondary Display)

| Component                       | ESP32-S3 Pin  | Function          | Notes                   |
| ------------------------------- | ------------- | ----------------- | ----------------------- |
| **ST7789V LCD Display**         |               |                   |                         |
| CS                              | GPIO45        | Chip Select       | SPI                     |
| DC                              | GPIO42        | Data/Command      | SPI                     |
| Reset                           | GPIO0         | Hardware Reset    | Active Low              |
| Backlight                       | GPIO1         | Backlight Control | PWM capable             |
| CLK                             | GPIO39        | SPI Clock         | Standard SPI            |
| MOSI                            | GPIO38        | SPI Data          | Standard SPI            |
| **ADC UV Sensor (Legacy)**      |               |                   |                         |
| OUT                             | GPIO4         | Analog Output     | ML8511 equivalent       |

### ESP32 Sensor Unit

| Component                       | ESP32 Pin     | Function          | Notes                   |
| ------------------------------- | ------------- | ----------------- | ----------------------- |
| **SHT31 Sensor**                |               |                   |                         |
| SDA                             | GPIO21        | I2C Data          | Pull-up required        |
| SCL                             | GPIO19        | I2C Clock         | Pull-up required        |
| VCC                             | 3.3V          | Power             |                         |
| GND                             | GND           | Ground            |                         |
| **DS18B20 Temperature Sensors** |               |                   |                         |
| Data                            | GPIO32        | OneWire Data      | Requires 10kΩ pull-up   |
| VCC                             | 3.3V          | Power             | Can use parasitic power |
| GND                             | GND           | Ground            |                         |
| **Pull-up Resistor**            |               |                   |                         |
| 10kΩ                            | GPIO32 ↔ 3.3V | OneWire Pull-up   | Essential for DS18B20   |
| **DS18B20 Temperature Sensors** |               |                   |                         |
| Data                            | GPIO11        | OneWire Data      | Requires 10kΩ pull-up   |
| VCC                             | 3.3V          | Power             | Can use parasitic power |
| GND                             | GND           | Ground            |                         |
| **External Fan**                |               |                   |                         |
| Control                         | GPIO21        | Digital Output    | Drive relay/MOSFET      |
| **Pull-up Resistor**            |               |                   |                         |
| 10kΩ                            | GPIO11 ↔ 3.3V | OneWire Pull-up   | Essential for DS18B20   |

## 🔌 Connector Types

### Power Connectors

- **USB-C** - Programming and power (ESP32-S3 DevKit)
- **External Power** - Optional 5V barrel jack or terminal block

### Sensor Connectors

- **JST-XH 2.54mm** - Recommended for removable sensors
- **Terminal Blocks** - Screw terminals for permanent connections
- **DuPont Connectors** - 2.54mm pitch for breadboard prototyping

### Waterproof Connections (for vivarium use)

- **DS18B20 Probes** - Often come with 3-wire cable and connector
- **IP65 Cable Glands** - For sensor wires entering enclosure

## 📦 Bill of Materials (BOM)

| Qty | Component                  | Part Number/Specs           | Estimated Cost |
| --- | -------------------------- | --------------------------- | -------------- |
| 2   | ESP32-S3 DevKit-C-1        | ESP32-S3-DevKitC-1-N8R8     | $15-20 each    |
| 1   | ESP32 DevKit               | ESP32-WROOM-32              | $8-12          |
| 1   | RM67162 AMOLED Display     | 240x400px QSPI              | $25-35         |
| 1   | ST7789V LCD Display        | 240x320px SPI               | $8-15          |
| 1   | SHT31 Sensor               | SHT31-DIS-B                 | $8-12          |
| 1   | LTR390 UV/Light Sensor     | LTR390 I2C Breakout         | $6-10          |
| 1   | HC-SR04 Ultrasonic Sensor  | Distance measurement        | $2-5           |
| 2   | DS18B20 Temperature Sensor | Waterproof probes           | $3-5 each      |
| 1   | Broadlink RM Mini 3        | IR blaster                  | $15-25         |
| 1   | 10kΩ Resistor              | 1/4W 5% tolerance           | $0.10          |
| 1   | TerraSky LED System        | Full spectrum reptile light | $50-100        |
| 3   | Smart WiFi Switches        | Fan/Mister/Fogger control   | $10-15 each    |
| 1   | PCB/Perfboard              | Custom or prototype board   | $5-15          |
| 1   | Enclosure                  | IP65 rated for vivarium use | $10-20         |
|     | Connectors & Wire          | JST, terminals, etc.        | $10-20         |
|     | **Total**                  |                             | **~$200-350**  |

## Step-by-Step Setup Instructions

### 1. Install ESPHome

1. **Install ESPHome** via Home Assistant Add-on Store or standalone:

   - Home Assistant → Add-ons → ESPHome
   - Or install via pip: `pip install esphome`

2. **Create New Device:**
   - ESPHome Dashboard → New Device
   - Choose ESP32-S3 as the platform
   - Set device name (e.g., "vivarium-monitor")

### 2. Hardware Assembly

1. **Connect all sensors** according to the pin connection table above
2. **Install 10kΩ pull-up resistor** between GPIO11 and 3.3V for DS18B20 sensors
3. **Test connections** with multimeter before powering on

### 3. ESPHome Configuration

1. **Copy the configuration** from `sandbox.yml`
2. **Update WiFi credentials** in your secrets file
3. **Find DS18B20 addresses:**
   - Comment out DS18B20 sensors initially
   - Upload and check logs for device addresses
   - Update configuration with real addresses

### 4. Upload Firmware

1. **Connect ESP32-S3** via USB
2. **Compile and upload** through ESPHome dashboard
3. **Monitor logs** for successful sensor detection

### 5. Testing & Troubleshooting

1. **Check ESPHome logs** for sensor readings and connectivity
2. **Verify Home Assistant integration** - entities should appear automatically
3. **Test display functionality** - should show live sensor data
4. **Common issues:**
   - **No DS18B20 detected?** Check 10kΩ pull-up resistor and wiring
   - **Display not working?** Verify SPI connections and power
   - **WiFi issues?** Check credentials in secrets.yaml
   - **Compilation errors?** Ensure ESPHome version compatibility

### 6. Customization

- Modify display layout and colors in the lambda section
- Add temperature thresholds and alerts
- Integrate additional sensors or controls
- Create Home Assistant automations based on sensor data

## 🏠 Home Assistant Integration

This project features comprehensive **ESPHome** integration with Home Assistant for complete vivarium automation:

### Sensors

- **Air Temperature** - SHT31 main vivarium air temperature
- **Humidity** - SHT31 humidity sensor with percentage readings
- **Basking Temperature** - DS18B20 probe for basking spot monitoring
- **Substrate Temperature** - DS18B20 probe for substrate/ground temperature
- **UV Index** - LTR390 UV index sensor (0-11+ scale)
- **Ambient Light** - LTR390 light sensor (lux measurements)
- **Water Level** - HC-SR04 ultrasonic distance sensor

### Controls & Automation

- **TerraSky Lighting** - IR controlled full spectrum LED lighting
  - 8 lighting modes (Off, Low/Mid/High Sun, Low/Mid/High Moon, Full)
  - State tracking via Home Assistant input_select
  - Automated day/night cycles
- **Environmental Controls**
  - Smart Fan - WiFi controlled ventilation
  - Smart Mister - Automated misting system
  - Smart Fogger - Humidity control system
- **Broadlink Integration** - IR blaster for lighting control

### Display Features

#### AMOLED Display (Primary)
- **Real-time UV monitoring** - Color-coded UV index display
- **Ambient light readings** - Lux measurements
- **Distance measurements** - Water level monitoring
- **Status indicators** - System health and connectivity

#### LCD Display (Secondary) 
- **Animated status indicators** - Real-time visual feedback
  - Wind animation when fan is active
  - Diagonal raindrops when mister is active  
  - Sweeping fog clouds when fogger is active
- **Temperature color coding** - Instant visual temperature assessment
  - 🟠 Amber: Under 27°C (too cool)
  - 🟢 Green: 27-28°C (optimal range)
  - 🔴 Red: Over 28°C (too hot)
- **Lighting status graphics** - Visual sun/moon indicators
- **Smooth animations** - 100ms update interval for fluid motion

## 📊 Sensor Specifications

| Sensor            | Range           | Accuracy    | Update Rate | Location |
| ----------------- | --------------- | ----------- | ----------- | -------- |
| SHT31 Temperature | -40°C to +125°C | ±0.3°C      | 1 second    | Sensor Unit |
| SHT31 Humidity    | 0-100% RH       | ±2% RH      | 1 second    | Sensor Unit |
| DS18B20 Basking   | -55°C to +125°C | ±0.5°C      | 5 seconds   | Sensor Unit |
| DS18B20 Substrate | -55°C to +125°C | ±0.5°C      | 5 seconds   | Sensor Unit |
| LTR390 UV Index   | 0-11+ UV Index  | ±10%        | 1 second    | AMOLED Unit |
| LTR390 Light      | 0-65535 lux     | ±10%        | 1 second    | AMOLED Unit |
| HC-SR04 Distance  | 2-400 cm        | ±3mm        | 2 seconds   | AMOLED Unit |

## 🔧 ESPHome Configuration Files

The project includes multiple configuration files:

### Primary Configurations
- **`esp_display_AMOLED.yml`** - ESP32-S3 AMOLED unit with LTR390 and HC-SR04
- **`esp_display_LCD.yaml`** - ESP32-S3 LCD unit with animated status display
- **`sensor_unit.yaml`** - ESP32 sensor unit with SHT31 and DS18B20 sensors

### Home Assistant Integration
- **`broadlink.yml`** - Broadlink RM Mini 3 IR blaster configuration
- **`scripts.yaml`** - TerraSky lighting control scripts
- **`automation.yml`** - Environmental control automations

### Key Features in Configurations
```yaml
# LTR390 UV/Light Sensor (I2C - eliminates WiFi interference)
sensor:
  - platform: ltr390
    address: 0x53
    gain: X3
    resolution: 18
    uv_index:
      name: "UV Index"
    light:
      name: "Light"

# HC-SR04 Distance Sensor for water level
sensor:
  - platform: ultrasonic
    trigger_pin: GPIO10
    echo_pin: GPIO11
    name: "Distance"
    timeout: 4m
    unit_of_measurement: "cm"

# DS18B20 Temperature Sensors (Fast Updates)
sensor:
  - platform: dallas_temp
    address: 0xe28a447c1f64ff28  # Substrate probe
    update_interval: 5s
  - platform: dallas_temp  
    address: 0x74057c7c1f64ff28  # Basking probe
    update_interval: 5s
```

### Finding Your DS18B20 Addresses

1. Upload the configuration with sensors commented out
2. Check ESPHome logs for: `Found DS18B20 device with address: 0x...`
3. Update the addresses in your configuration
4. Re-upload the firmware

## Hardware Connections (ESPHome Configuration)

**Display (QSPI DBI):**

```
CS: GPIO6
Reset: GPIO17
Enable: GPIO38
CLK: GPIO47
Data Pins: [GPIO18, GPIO7, GPIO48, GPIO5]
```

**I2C Sensors:**

```
SDA: GPIO13 (SHT31)
SCL: GPIO14 (SHT31)
```

**OneWire Temperature:**

```
Data: GPIO11 (DS18B20 sensors with 10kΩ pull-up)
```

**Analog/Digital:**

```
UV Sensor: GPIO10 (ADC)
Fan Control: GPIO21 (Digital Output)
```

## Features

✅ **Dual ESP32-S3 Displays** - AMOLED and LCD with different functions  
✅ **Advanced Sensor Suite** - Temperature, humidity, UV, light, distance monitoring  
✅ **Smart Lighting Control** - IR controlled TerraSky with 8 lighting modes  
✅ **Animated Status Display** - Real-time visual feedback for all systems  
✅ **Temperature Color Coding** - Instant visual assessment of thermal conditions  
✅ **ESPHome Integration** - Direct Home Assistant connectivity  
✅ **WiFi Connectivity** - Wireless sensor monitoring and control  
✅ **Environmental Automation** - Fan, mister, fogger control  
✅ **Water Level Monitoring** - Ultrasonic distance measurement  
✅ **UV Index Monitoring** - Proper reptile lighting assessment  
✅ **Fast Sensor Updates** - Responsive 1-5 second update intervals  
✅ **Automatic Discovery** - Sensors appear in Home Assistant automatically  
✅ **State Tracking** - Complete system status monitoring  
✅ **IR Remote Learning** - Broadlink integration for lighting control  

## System Architecture

### Three-Unit Design

1. **ESP32-S3 AMOLED Unit** (Primary)
   - UV/Light monitoring with LTR390
   - Water level monitoring with HC-SR04
   - High-resolution environmental data display
   - WiFi enabled for real-time updates

2. **ESP32-S3 LCD Unit** (Secondary) 
   - Animated status display for all systems
   - Temperature color coding for quick assessment
   - Real-time visual feedback (wind, rain, fog animations)
   - Home Assistant sensor data integration

3. **ESP32 Sensor Unit** (Dedicated)
   - SHT31 air temperature and humidity
   - Dual DS18B20 temperature probes
   - Optimized for fast sensor readings (5-second updates)
   - Dedicated sensor processing

### Smart Home Integration

- **Broadlink RM Mini 3** - IR control hub for TerraSky lighting
- **Home Assistant** - Central automation and data logging
- **WiFi Smart Switches** - Environmental control (fan/mister/fogger)
- **State Management** - Input selects for lighting and system status

## Vivarium Monitoring Features

### Multi-Zone Temperature Monitoring

- **Basking Area** (DS18B20) - Critical for reptile thermoregulation
- **Substrate Level** (DS18B20) - Important for plant health and ground heat
- **Air Temperature** (SHT31) - Overall vivarium climate monitoring
- **Color-Coded Display** - Instant visual assessment:
  - 🟠 **Under 27°C** - Too cool, increase heating
  - 🟢 **27-28°C** - Optimal temperature range
  - 🔴 **Over 28°C** - Too hot, increase ventilation

### Advanced Environmental Controls

- **Humidity Management** - SHT31 monitoring with automated misting
- **UV Index Monitoring** - LTR390 ensures proper reptile lighting
- **Water Level Monitoring** - HC-SR04 prevents dry conditions
- **Smart Lighting Control** - TerraSky 8-mode system:
  - Off, Low/Mid/High Sun, Low/Mid/High Moon, Full spectrum
- **Animated Status Feedback** - Real-time visual system status:
  - Wind lines when fan is active
  - Diagonal raindrops when mister is running
  - Sweeping fog clouds when fogger is operating

### Automation Features

- **Day/Night Lighting Cycles** - Automated TerraSky control
- **Temperature-Based Ventilation** - Smart fan control
- **Humidity-Triggered Misting** - Automated moisture management
- **Status Logging** - Complete environmental data history
- **Alert Systems** - Home Assistant notifications for out-of-range conditions

## Next Steps

1. **Build the three hardware units** following the pin connection guides
2. **Set up Home Assistant** with ESPHome integration
3. **Configure Broadlink RM Mini 3** for IR lighting control
4. **Upload ESPHome configurations** to each unit
5. **Test sensor functionality** and calibrate readings
6. **Set up lighting automation** with TerraSky integration
7. **Create environmental automations** for optimal vivarium conditions
8. **Install in weatherproof enclosures** for vivarium deployment

## Troubleshooting Tips

### Common Issues

- **LTR390 Not Detected** - Check 3.3V power connection (common failure point)
- **DS18B20 Timeout** - Verify 10kΩ pull-up resistor on GPIO32
- **Display Issues** - Confirm SPI/QSPI pin connections
- **WiFi Problems** - Verify secrets.yaml credentials
- **Animation Lag** - Ensure 100ms display update interval for smooth motion

### Sensor Address Discovery

1. Upload configuration with DS18B20 sensors commented out
2. Check ESPHome logs for: `Found DS18B20 device with address: 0x...`
3. Update addresses in sensor_unit.yaml:
   - `0xe28a447c1f64ff28` - Substrate temperature probe
   - `0x74057c7c1f64ff28` - Basking temperature probe
4. Re-upload firmware with correct addresses

This comprehensive system provides professional-grade vivarium monitoring with advanced automation, visual feedback, and complete Home Assistant integration for optimal reptile care.
