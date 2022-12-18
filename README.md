# esphome-spapower
To keep full insight in the Power that the Jacuzzi (Spa bath) uses I have a 3-phase electricity meter for this load.
Eastron SDM meters are supported by ESPhome - so this should be a simple task.

The ESP is programmed with [ESPhome](https://esphome.io/), where it makes the data available on the built in web-interface, Exposing to Prometheus as well as publishing the metrics to a MQTT and available in HomeAssistant.

I had to define some new addresses in the config to get the 3-phase values I was interested in.

## Hardware
The main part here is the electricity meter.
I bought mine in advance - and got it connected by the electricity company when they were here and connected the wires to the jacuzzi.
I did need the 3-phase meter as the jacuzzi connects to different phases for different things. Different pumps and heaters are distributed on different phases to avoid overloading a single phase.
I have a 12V net, and needed a DC/DC to lower the voltage to 5V.
RS485-TTL converter to convert the RS-485 (Modbus) signals to TTL levels.

### Parts
D1 Mini - ESP8266 12-F board
[Eastron SDM630 Modbus MID v2 - 3-phase meter](https://www.energibutiken.se/sv/elmatare/223-3-fas-elmatare-for-dinskena-eastron-sdm630-modbus-mid-01004.html)
[RS485 to TTL level converter](https://www.amazon.se/ZkeeShop-adapter-seriell-UART-niv%C3%A5-omvandlarmodul/dp/B0716VF1CC/ref=asc_df_B0716VF1CC/?tag=shpngadsglede-21&linkCode=df0&hvadid=476437602690&hvpos=&hvnetw=g&hvrand=14483052703191124037&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=21011&hvtargid=pla-683366003419&psc=1)


### Wiring D1Mini
#### Breadboard Testing

I have some more images and a wiring diagram as well - somewhere.
Ping me if you want it - and I'll dig it up.

## Installation
Clone the repository and create a companion `secrets.yaml` file with the following fields:
```
wifi_ssid: <your wifi SSID>
wifi_password: <your wifi password>
solartemp_password: <Your p1mini password (for OTA, API, etc)>
```
Make sure to place the `secrets.yaml` file in the root path of the cloned project. The `p1mini_password` field can be set to any password before doing the initial upload of the firmware.

Flash ESPHome as usual, just keep the `p1mini.h` file in the same location as `p1mini.yaml` (and `secrets.yaml`). *Don't* connect USB and the P1 port at the same time! If everything works, Home Assistant will autodetect the new integration after you plug it into the P1 port.

If you do not receive any data, make sure that the P1 port is enabled on your meter and try setting the log level to `DEBUG` in ESPHome for more feedback.

## Technical documentation
Specification overview:
https://esphome.io/
https://www.amazon.se/AZDelivery-D1-Mini-ESP8266-12F-WLAN-modul/dp/B0754W6Z2F?th=1
https://esphome.io/components/sensor/sdm_meter.html#
http://www.eastrongroup.com/product_detail.php?id=143&menu1=36&menu2=52
