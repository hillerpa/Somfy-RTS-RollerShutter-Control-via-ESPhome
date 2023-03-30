# Somfy-RTS-RollerShutter-Control-via-ESPhome
This is mainly based on the [articel](https://www.die-welt.net/2021/06/controlling-somfy-roller-shutters-using-an-esp32-and-esphome/) but I had some issues and adopted on some points as well as documented it for my needs.

## Roller Shutter sniffing
### Hardware required:
* Ubuntu 20.04 Distribution or something similar
* RTL-SDR Sniffer e.g. [Sniffer](https://de.aliexpress.com/item/32903103541.html?spm=a2g0o.order_list.order_list_main.17.77735c5fvztw8b&gatewayAdapt=glo2deu)

### Basic Software required (from blank ubuntu):
* net-tools for IP detection and ssh access
* git for git Data
* build-essential for make
* rtl-sdr to detect the HW
```
sudo apt install net-tools
sudo apt install ssh
sudo apt install git
sudo apt install build-essential
sudo apt install rtl-sdr
```

### RTL-SDR Preparation:
* Clone Repository
* make C Files
```
git clone https://github.com/dimhoff/radio_stuff
cd radio_stuff/
make -C converters am_to_ook
make -C decoders decode_somfy
```

### RTL-SDR Sniffing:
```
sudo  rtl_fm -M am -f 433.42M -s 270K | converters/am_to_ook -d 10 -t 2000 -  | decoders/decode_somfy
```

## Hardware Preparation
* [ESP8266](https://de.aliexpress.com/item/1005004527993328.html?spm=a2g0o.productlist.main.1.d0a151fe22qfFc&algo_pvid=a588ff3a-3467-4a2d-a764-ee6ce13e0b89&algo_exp_id=a588ff3a-3467-4a2d-a764-ee6ce13e0b89-0&pdp_ext_f=%7B%22sku_id%22%3A%2212000030765703491%22%7D&pdp_npi=3%40dis%21EUR%215.17%213.62%21%21%21%21%21%40212240a316781279882565728d0711%2112000030765703491%21sea%21DE%213245007737&curPageLogUid=QT3H20LpoWIO) or derivate
* [TI-CC1101](https://de.aliexpress.com/item/1005002074380868.html?spm=a2g0o.productlist.main.1.495e384d7G3QKO&algo_pvid=842d5d78-f38d-4464-9a5d-f1b6ad3076e7&algo_exp_id=842d5d78-f38d-4464-9a5d-f1b6ad3076e7-0&pdp_ext_f=%7B%22sku_id%22%3A%2212000025798608305%22%7D&pdp_npi=3%40dis%21EUR%214.58%213.16%21%21%21%21%21%40211bf49716781278115932071d0720%2112000025798608305%21sea%21DE%213245007737&curPageLogUid=uNAAKaRTs9De)

## Wiring
![grafik](https://user-images.githubusercontent.com/94315369/228869506-c1ff1c20-7c00-40ae-8f1c-30a03cb7470a.png)

I use the NodeMCU V3 which have the same config as the D1 mini. and the new TI-CC1101 design:
![grafik](https://user-images.githubusercontent.com/94315369/223201040-6bc8d93c-c56f-41c4-9ee1-19ed87922f40.png)

| ESP32      | TI CC1101 |
| ----------- | ----------- |
| GPIO2      | GD00 3       |
| GPIO4   | GD02 8        |
| GPIO18   | SCK 5        |
| 3V3   | VCC 2        |
| GPIO23   | MOSI 6        |
| GPIO19   | MISO 7        |
| GPIO5   | CSN/CKN 4        |
| GND   | GND 1        |


## ESPHome Software Config
* Install in HomeAssistant Settings->Addon -> ESPHome 
* Connect ESP8266 to the PC for initial Flashing
  * I used https://web.esphome.io/ with initial Flashing to get the ESP8266 into HomeAssistant



