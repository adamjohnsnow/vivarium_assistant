# Waveshare ESP32-S3 AMOLED 1.91" with Home Assistant Integration

## Step-by-Step Setup Instructions

### 1. Install Arduino IDE and Libraries

1. **Install Arduino IDE 2.x** from https://www.arduino.cc/en/software

2. **Add ESP32 Board Support:**

   - File → Preferences
   - Add to "Additional Board Manager URLs":
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Tools → Board → Boards Manager → Search "ESP32" → Install "ESP32 by Espressif Systems"

3. **Install Required Libraries:**
   - Tools → Manage Libraries
   - Install these libraries:
     - `PubSubClient` by Nick O'Leary
     - `ArduinoJson` by Benoit Blanchon
     - `WiFi` (included with ESP32)

### 2. Download Complete Display Drivers

**IMPORTANT:** The files I created are basic implementations. For full functionality, you need to download the complete drivers from the GitHub repository:

1. Go to: https://github.com/EDISON-SCIENCE-CORNER/WAVESHARE-1.9-AMOLED/tree/main/RGB%20LED%20CONTROLLER
2. Download these files and replace the basic versions:
   - `rm67162.h`
   - `rm67162.cpp`
   - `FT3168.h`
   - `FT3168.cpp`
   - `Display.h`
   - Font files (\*.h)

### 3. Configure the Sketch

1. **Open the sketch:** `amoled_homeassistant.ino`

2. **Update WiFi credentials:**

   ```cpp
   const char* ssid = "YOUR_WIFI_SSID";
   const char* password = "YOUR_WIFI_PASSWORD";
   ```

3. **Update MQTT settings:**
   ```cpp
   const char* mqtt_server = "192.168.1.100";  // Your Home Assistant IP
   const int mqtt_port = 1883;
   const char* mqtt_user = "your_mqtt_user";
   const char* mqtt_password = "your_mqtt_password";
   ```

### 4. Upload to ESP32-S3

1. **Select Board:**

   - Tools → Board → ESP32 Arduino → ESP32S3 Dev Module

2. **Configure Board Settings:**

   - CPU Frequency: 240MHz
   - Flash Size: 4MB (or 16MB if you have that version)
   - Partition Scheme: Default 4MB with spiffs
   - PSRAM: Enabled

3. **Connect your ESP32-S3** via USB and select the correct port

4. **Upload the sketch**

### 5. Home Assistant Configuration

Add this to your `configuration.yaml`:

```yaml
mqtt:
  sensor:
    - name: "AMOLED Display Status"
      state_topic: "homeassistant/amoled/status"
      value_template: "{{ value_json.status }}"
      json_attributes_topic: "homeassistant/amoled/status"

    - name: "AMOLED Touch Events"
      state_topic: "homeassistant/amoled/touch"
      value_template: "{{ value_json.button }}"
      json_attributes_topic: "homeassistant/amoled/touch"

  light:
    - name: "AMOLED Display"
      command_topic: "homeassistant/amoled/command"
      brightness_command_topic: "homeassistant/amoled/brightness"
      brightness_scale: 255
      payload_on: "toggle"
      payload_off: "toggle"

  text:
    - name: "AMOLED Display Text"
      command_topic: "homeassistant/amoled/text"
      initial: "Home Assistant"
```

### 6. Testing

1. **Monitor Serial Output** to see connection status and debug info
2. **Check Home Assistant** for the new entities
3. **Test touch buttons** on the display
4. **Send commands** from Home Assistant to control the display

### 7. Troubleshooting

- **Display not working?** Make sure you have the complete rm67162.cpp from GitHub
- **Touch not responding?** Check I2C connections and addresses
- **MQTT issues?** Verify broker settings and credentials
- **Compilation errors?** Ensure all libraries are installed and Arduino includes are present
- **String conversion errors?** Use `.toString()` for IP addresses and `String()` for integers

### 8. Customization

- Modify button layouts in the `buttons[]` array
- Add more MQTT topics for additional sensors
- Customize display graphics and fonts
- Add more touch gestures and interactions

## Hardware Connections

The pin configuration is defined in `pins_config.h`:

```
Display SPI:
- MOSI: GPIO18
- SCLK: GPIO47
- CS: GPIO6
- DC: GPIO7
- RST: GPIO17
- BL: GPIO38

Touch I2C:
- SDA: GPIO15
- SCL: GPIO16
- INT: GPIO1
- RST: GPIO14
```

## Features

✅ WiFi connectivity  
✅ MQTT communication with Home Assistant  
✅ Touch screen interface with buttons  
✅ Brightness control  
✅ Text display from Home Assistant  
✅ Status reporting  
✅ Touch event publishing

## Next Steps

1. Get the complete display drivers from GitHub
2. Test basic functionality first
3. Add your specific sensors and features
4. Create custom Home Assistant dashboards
5. Add more sophisticated graphics and animations
