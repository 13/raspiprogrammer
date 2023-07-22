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

## Cmds
```
flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=512
flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=512 -r [ oldbios]
flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=512 -w [newbios]

flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768
flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=16000
```

## Script
```
#!/bin/bash

# Function to display the help message
show_help() {
  echo "Usage: $0 <bios_name> <count>"
  echo "Generate BIOS files with a given name and number, then compute SHA1 checksum."
  echo "Parameters:"
  echo "  <bios_name>    : The desired name for the BIOS files."
  echo "  <count>        : The number of BIOS files to generate (e.g., 3 will generate bios1.bin, bios2.bin, bios3.bin)."
}

if [ "$#" -eq 0 ] || [ "$1" == "--help" ] || [ "$1" == "-h" ]; then
  show_help
  exit 0
fi

if [ "$#" -ne 2 ]; then
  echo "Error: Incorrect number of arguments."
  show_help
  exit 1
fi

bios_name="$1"
count="$2"

for ((i = 1; i <= count; i++)); do
  filename="${bios_name}${i}.bin"
  flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768 -r "$filename"
done

sha1sum "${bios_name}"*.bin
```
