# esphome-spapower
To keep full insight in the power that the Jacuzzi (Spa bath) uses I have a 3-phase electricity meter for measuring the load.
Eastron SDM meters are supported by ESPhome - so this should be a simple task.

The ESP is programmed with [ESPhome](https://esphome.io/), where it makes the data available on the built in web-interface, exposing to Prometheus as well as publishing the metrics to MQTT and to Home Assistant via the native API.

I had to define some new addresses in the config to get the 3-phase values I was interested in.

## Hardware
The main part here is the electricity meter.

I bought mine in advance - and got it connected by the electricity company when they were here and connected the wires to the jacuzzi.

I did need the 3-phase meter as the jacuzzi connects to different phases for different things. Different pumps and heaters are distributed on different phases to avoid overloading a single phase.

I have a 12V net, and needed a DC/DC to lower the voltage to 5V.
RS485-TTL converter to convert the RS-485 (Modbus) signals to TTL levels.

### Parts
* D1 Mini - ESP8266 12-F board
* [Eastron SDM630 Modbus MID v2 - 3-phase meter](https://www.energibutiken.se/sv/elmatare/223-3-fas-elmatare-for-dinskena-eastron-sdm630-modbus-mid-01004.html)
* [RS485 to TTL level converter](https://www.amazon.se/ZkeeShop-adapter-seriell-UART-niv%C3%A5-omvandlarmodul/dp/B0716VF1CC/ref=asc_df_B0716VF1CC/?tag=shpngadsglede-21&linkCode=df0&hvadid=476437602690&hvpos=&hvnetw=g&hvrand=14483052703191124037&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=21011&hvtargid=pla-683366003419&psc=1)


### Wiring D1Mini
#### Breadboard Testing

I have some more images and a wiring diagram as well - somewhere.
Ping me if you want it - and I'll dig it up.

## Installation
Clone the repository and create a companion `secrets.yaml` file with the following fields:
```
wifi_ssid: <your wifi SSID>
wifi_password: <your wifi password>
spapower_password: <Your device password (for OTA, AP fallback, etc)>
api_encr_b64key: <a base64-encoded 32-byte key for the Home Assistant API encryption>
```
Make sure to place the `secrets.yaml` file in the root path of the cloned project.

Flash the firmware with ESPHome as usual:
```
esphome run spapower.yaml
```
On the first flash connect the D1 Mini over USB; afterwards updates can be pushed Over-The-Air (OTA) over WiFi.

Once running, Home Assistant will autodetect the device via the native ESPHome API once it is connected to the Eastron meter via Modbus. The sensor values are also published to MQTT (under the `spapower` topic prefix, with MQTT discovery disabled to avoid duplicating the entities already provided by the native API) and are available on the built-in web interface (port 80) and the Prometheus endpoint.

If you do not receive any data, double check the Modbus wiring (RS485-TTL converter to the D1 Mini's UART pins) and try setting the log level to `DEBUG` in ESPHome for more feedback.

## Testing
Monitor what's being sent to the MQTT bus with the following command:
```mosquitto_sub -h 192.150.23.16 -p 1883 -t spapower/+/# -v```

## Technical documentation
Specification overview:
* http://www.eastrongroup.com/product_detail.php?id=143&menu1=36&menu2=52
* https://esphome.io/
* https://esphome.io/components/sensor/sdm_meter.html#
* https://www.amazon.se/AZDelivery-D1-Mini-ESP8266-12F-WLAN-modul/dp/B0754W6Z2F?th=1
