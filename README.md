# ESPHome Custom RGBWT Light Component

Custom ESPHome light component for lights that use white brightness and colour temperature channels (rather than Cold White and Warm White) 

Example for a [InLine SmartHome LED bulb RGB E27](https://www.inline-info.com/en/products/smart-home/11207/inline-smarthome-led-bulb-rgb-e27).

```yaml
esphome:
  devicename: rgbwt_001
  platform: ESP8266
  board: esp01_1m
  includes:
    - light-rgbwt.h

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: rgbwt_001
    password: somethingVerySecure!

captive_portal:

# Enable logging to ESPHome
logger:
  # Disable logging to serial
  baud_rate: 0

# Enable Home Assistant API
api:
  password: !secret ha_api_password

ota:
  password: !secret ota_password

output:
  - platform: esp8266_pwm
    id: output_red
    pin: GPIO14
  - platform: esp8266_pwm
    id: output_green
    pin: GPIO12
  - platform: esp8266_pwm
    id: output_blue
    pin: GPIO5
  - platform: esp8266_pwm
    id: output_brightness
    pin: GPIO15
  - platform: esp8266_pwm
    id: output_color_temp
    pin: GPIO13

light:
  - platform: custom
    lambda: |-
      auto light_out = new RGBWT(id(output_red), id(output_green), id(output_blue), id(output_brightness), id(output_color_temp));
      App.register_component(light_out);
      return {light_out};
    lights:
      - name: "RGBWT Light"
        restore_mode: RESTORE_DEFAULT_ON
```

All thanks to @jesserockz and @EuroTrash on [ESPHome Discord](https://discord.gg/KhAMKrd) and [original repo by dieselrabbit](https://github.com/dieselrabbit/FeitGen2_ESPHome).

## #TODO

- pass in warm and cold colour temperatures (replace hardcoded values)
