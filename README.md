# Home Assistant support for Tuya BLE devices

## Overview

This integration supports Tuya devices connected via BLE.

_Forked from [@PlusPlus-ua](https://github.com/PlusPlus-ua/ha_tuya_ble) and [@jbsky](https://github.com/jbsky/ha_tuya_ble)._

## Installation

Place the `custom_components` folder in your configuration directory (or add its contents to an existing `custom_components` folder). Alternatively install via [HACS](https://hacs.xyz/).

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=deldrid1&repository=ha_tuya_ble&category=integration)

## Usage

After adding to Home Assistant integration should discover all supported Bluetooth devices, or you can add discoverable devices manually.

The integration works locally, but connection to a Tuya BLE device requires the device ID and encryption key from Tuya IoT cloud. Almost all devices only need cloud access during setup.

You can authorize setup in either of two ways:

* Tuya app account mode: enter the IoT Access ID, IoT Access Secret, country, Tuya Smart or Smart Life account, and password.
* IoT project device ID mode: enter the IoT Access ID, IoT Access Secret, country, and one or more comma-separated Tuya device IDs that are already authorized in the IoT project. Leave the app account and password blank.

## Bluetooth proxy support

Tuya BLE control requires an active BLE GATT connection. [Home Assistant's Bluetooth integration](https://www.home-assistant.io/integrations/bluetooth/#remote-adapters-bluetooth-proxies) supports active connections through local Bluetooth adapters and [ESPHome Bluetooth proxies](https://esphome.io/components/bluetooth_proxy/).

Shelly Gen2+ Bluetooth proxy support is limited to advertisement listening and advertisement bundling, not active GATT connections. A Shelly device can help Home Assistant see Tuya BLE advertisements, but it cannot proxy the active connection needed to read or write Tuya BLE datapoints. Use an ESPHome Bluetooth proxy with active connections enabled, or a supported local Bluetooth adapter, for controllable Tuya BLE devices.

Some Wi-Fi + Bluetooth Tuya combo modules only advertise their Tuya Bluetooth service while they are in pairing mode or Wi-Fi fallback mode. This fork listens for both the older `0000a201-0000-1000-8000-00805f9b34fb` service and the newer TuyaOS `0000fd50-0000-1000-8000-00805f9b34fb` advertising service. FD50 devices use TuyaOS GATT characteristics `00000002-0000-1001-8001-00805f9b07d0` for notifications and `00000001-0000-1001-8001-00805f9b07d0` for writes.

## Supported devices list

* Backyard Discovery Sauna Heater (category_id 'dj')
  + BYD Sauna Heater / product_id '0envtxjyq7wn7h6t'.
  + Exposes a climate entity in Fahrenheit, controls power switch, setpoint temperature number, timer number, current temperature sensor, countdown-left sensor, temperature-unit select, display-mode select, a fault binary sensor, and a disabled-by-default BLE signal-strength diagnostic sensor. The unwired light output is intentionally not exposed.
  + The climate entity reports `heat` only while the timer is active. Setting the climate mode to `heat` starts a 60 minute timer; setting it to `off` clears the timer.
  + Known datapoints:
    - 20 `switch_led`: heater power
    - 21 `work_mode`: hidden app display mode (`hide`, `bright`, `temp`, `countdown`)
    - 26 `countdown`: timer setpoint, 0-60 min, 5 min step
    - 101 `countdown_left`: remaining timer minutes
    - 102 `temp_set`: Celsius target, 0-90 C, 5 C step
    - 103 `temp_current`: current Celsius temperature
    - 104 `temp_unit_convert`: `c`/`f`
    - 105 `temp_current_f`: current Fahrenheit temperature
    - 106 `temp_set_f`: Fahrenheit target, 32-194 F, 9 F step
    - 107 `fault`: bitmap (`f01` sensor contact fault, `f03` 125 C over-temperature alarm)
    - 108 `brightness`: unwired light output, intentionally not exposed

* Fingerbots (category_id 'szjqr')
  + Fingerbot (product_ids 'ltak7e1p', 'y6kttvd6', 'yrnk7mnn', 'nvr2rocq', 'bnt7wajf', 'rvdceqjh', '5xhbk964'), original device, first in category, powered by CR2 battery.
  + Adaprox Fingerbot (product_id 'y6kttvd6'), built-in battery with USB type C charging.
  + Fingerbot Plus (product_ids 'blliqpsj', 'ndvkgsrm', 'yiihr7zh', 'neq16kgd', 'mknd4lci', 'riecov42'), almost same as original, has sensor button for manual control.
  + CubeTouch 1s (product_id '3yqdo5yt'), built-in battery with USB type C charging.
  + CubeTouch II (product_id 'xhf790if'), built-in battery with USB type C charging.

  All features available in Home Assistant, programming (series of actions) is implemented for Fingerbot Plus.
  For programming exposed entities 'Program' (switch), 'Repeat forever', 'Repeats count', 'Idle position' and 'Program' (text). Format of program text is: 'position\[/time\];...' where position is in percents, optional time is in seconds (zero if missing).

* Temperature and humidity sensors (category_id 'wsdcg')
  + Soil moisture sensor (product_id 'ojzlzzsw').

* CO2 sensors (category_id 'co2bj')
  + CO2 Detector (product_id '59s19z5m').

* Smart Locks (category_id 'ms')
  + Smart Lock (product_id 'ludzroix', 'isk2p555').

* Climate (category_id 'wk')
  + Thermostatic Radiator Valve (product_ids 'drlajpqc', 'nhj2j7su').

* Smart water bottle (category_id 'znhsb')
  + Smart water bottle (product_id 'cdlandip')

* Irrigation computer (category_id 'ggq')
  + Irrigation computer (product_ids '6pahkcau', 'hfgdqhho')
  + Irrigation computer - Jectse (product_id 'fnlw6npo')

* Water valve (category_id 'sfkzq')
  + Water valve (product_id 'nxquc5lb')

## Support project

I am working on this integration in Ukraine. Our country was subjected to brutal aggression by Russia. The war still continues. The capital of Ukraine - Kyiv, where I live, and many other cities and villages are constantly under threat of rocket attacks. Our air defense forces are doing wonders, but they also need support. So if you want to help the development of this integration, donate some money and I will spend it to support our air defense.
<br><br>
<p align="center">
  <a href="https://www.buymeacoffee.com/3PaK6lXr4l"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy me an air defense"></a>
</p>
