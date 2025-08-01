# Vivarium Assistant - ESP32-S3 AMOLED Environmental Monitor

A comprehensive environmental monitoring system for vivariums using ESP32-S3 with AMOLED display and multiple sensors.

## 🔧 Hardware Components

### Main Components

- **ESP32-S3 DevKit-C-1** - Main microcontroller with WiFi
- **RM67162 AMOLED Display** - 240x400px color display
- **SHT31 Temperature/Humidity Sensor** - I2C environmental sensor
- **ML8511 UV Sensor** - Analog UV light measurement
- **2x DS18B20 Temperature Sensors** - OneWire waterproof probes
- **10kΩ Resistor** - Pull-up resistor for OneWire bus
- **External Fan Control** - GPIO-controlled relay/MOSFET

### Power Requirements

- **Input:** 5V USB or external power supply
- **Operating:** 3.3V (regulated on ESP32-S3 board)
- **Current:** ~200-500mA (depending on display brightness and sensors)

## 📡 Pin Connections

| Component                       | ESP32-S3 Pin  | Function          | Notes                   |
| ------------------------------- | ------------- | ----------------- | ----------------------- |
| **AMOLED Display (RM67162)**    |               |                   |                         |
| CS                              | GPIO6         | Chip Select       | SPI                     |
| Reset                           | GPIO17        | Hardware Reset    | Active Low              |
| Enable                          | GPIO38        | Display Enable    | Backlight Control       |
| CLK                             | GPIO47        | SPI Clock         | Quad SPI                |
| Data[0]                         | GPIO18        | SPI Data 0        | QSPI                    |
| Data[1]                         | GPIO7         | SPI Data 1        | QSPI                    |
| Data[2]                         | GPIO48        | SPI Data 2        | QSPI                    |
| Data[3]                         | GPIO5         | SPI Data 3        | QSPI                    |
| **SHT31 Sensor**                |               |                   |                         |
| SDA                             | GPIO13        | I2C Data          | Pull-up required        |
| SCL                             | GPIO14        | I2C Clock         | Pull-up required        |
| VCC                             | 3.3V          | Power             |                         |
| GND                             | GND           | Ground            |                         |
| **ML8511 UV Sensor**            |               |                   |                         |
| OUT                             | GPIO10        | Analog Output     | ADC input               |
| VCC                             | 3.3V          | Power             |                         |
| GND                             | GND           | Ground            |                         |
| EN                              | 3.3V          | Enable (optional) | Always enabled          |
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
| 1   | ESP32-S3 DevKit-C-1        | ESP32-S3-DevKitC-1-N8R8     | $15-20         |
| 1   | RM67162 AMOLED Display     | 240x400px QSPI              | $25-35         |
| 1   | SHT31 Sensor               | SHT31-DIS-B                 | $8-12          |
| 1   | ML8511 UV Sensor           | ML8511 Breakout             | $5-8           |
| 2   | DS18B20 Temperature Sensor | Waterproof probes           | $3-5 each      |
| 1   | 10kΩ Resistor              | 1/4W 5% tolerance           | $0.10          |
| 1   | PCB/Perfboard              | Custom or prototype board   | $5-15          |
| 1   | Enclosure                  | IP65 rated for vivarium use | $10-20         |
|     | Connectors & Wire          | JST, terminals, etc.        | $5-10          |
|     | **Total**                  |                             | **~$80-130**   |

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

This project uses **ESPHome** for seamless Home Assistant integration. The configuration includes:

### Sensors

- **Vivarium Temperature** - SHT31 main air temperature
- **Vivarium Humidity** - SHT31 humidity sensor
- **Water Temperature** - DS18B20 probe (0xe28a447c1f64ff28)
- **Substrate Temperature** - DS18B20 probe (0x74057c7c1f64ff28)
- **UV Level** - ML8511 UV index sensor

### Controls

- **External Fan** - GPIO21 controlled ventilation fan
- **Lamp Status** - Home Assistant entity monitoring

### Display Features

- **Real-time monitoring** - Live sensor data on AMOLED screen
- **Color-coded alerts** - Green/red indicators for optimal ranges
- **Status indicators** - Fan, lamp, and system status

## 📊 Sensor Specifications

| Sensor            | Range           | Accuracy    | Update Rate |
| ----------------- | --------------- | ----------- | ----------- |
| SHT31 Temperature | -40°C to +125°C | ±0.3°C      | 1 second    |
| SHT31 Humidity    | 0-100% RH       | ±2% RH      | 1 second    |
| DS18B20 Water     | -55°C to +125°C | ±0.5°C      | 60 seconds  |
| DS18B20 Substrate | -55°C to +125°C | ±0.5°C      | 60 seconds  |
| ML8511 UV         | 280-390nm       | ±1 UV Index | 2 seconds   |

## 🔧 ESPHome Configuration

The main configuration file `sandbox.yml` includes:

```yaml
# Key sensor addresses (update these for your hardware)
sensor:
  - platform: dallas_temp
    address: 0xe28a447c1f64ff28 # Water temperature probe
  - platform: dallas_temp
    address: 0x74057c7c1f64ff28 # Substrate temperature probe
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

✅ **ESPHome Integration** - Direct Home Assistant connectivity  
✅ **WiFi Connectivity** - Wireless sensor monitoring  
✅ **Multi-Sensor Monitoring** - Temperature, humidity, UV levels  
✅ **AMOLED Display** - High-contrast color display with live data  
✅ **Color-Coded Alerts** - Visual indicators for optimal conditions  
✅ **Automatic Discovery** - Sensors appear in Home Assistant automatically  
✅ **Real-time Updates** - Live environmental data  
✅ **Fan Control** - GPIO-controlled ventilation

## Vivarium Monitoring Features

### Temperature Zones

- **Main Air Temperature** (SHT31) - Overall vivarium climate
- **Water Temperature** (DS18B20) - Critical for aquatic sections
- **Substrate Temperature** (DS18B20) - Important for plant health

### Environmental Controls

- **Humidity Monitoring** - Maintain optimal moisture levels
- **UV Level Detection** - Monitor lighting conditions
- **Fan Control** - Automated ventilation based on conditions
- **Lamp Status** - Integration with Home Assistant lighting

## Next Steps

1. **Build the hardware** following the pin connection guide
2. **Test basic functionality** with simple ESPHome config
3. **Add your specific sensors** and calibrate readings
4. **Create Home Assistant automations** for environmental control
5. **Mount in weatherproof enclosure** for vivarium use
