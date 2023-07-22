# raspiprogrammer
Raspberry Pi SPI Flash ROM Programmer

## Configuration
Edit /boot/config.txt
- Enable SPI 
```
dtparam=spi=on
```
## Connection
| Name | 25 SPI | RasPi |
|------|--------|-------|
| CS/CE0 | 1 | 24 |
| MISO | 2 | 21 |
| GND | 4 | 20 |
| MOSI | 5 | 19 |
| SCLK | 6 | 23 |
| VCC | 8 | 17 |
