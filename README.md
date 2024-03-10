# DWIN DMG80480Y070_02NN
First impressions of the Dwin display model DMG80480Y070_02NN and Basic commands

If you are looking for a low-cost display and are targeting an [SSD1963](https://github.com/rtek1000/Display_TFT_800x480_7_inch_SSD1963), it might be interesting to check out Dwin displays.

-----

- [Danger: high voltage for ESP32 and others below 5V](https://github.com/dwinhmi/DWIN_DGUS_HMI/issues/17)
- - The DMG80480Y070_02NN display is releasing 5V at output TX2.
- - Datasheet says: TX2 output 3.2V, but my ESP32 started receiving data from the display intermittently.
  
-----

This model DMG80480Y070_02NN is 800x480 in 7 inches, with only additional Buzzer and communication connector.

- Communication via serial 8N1 [TTL(default)/RS232 selectable via jumper (soldered on the board)]
- Can make the buzzer beep
- Can control Backlight
- Has CRC16 error control (Modbus style with swapped bytes)
- This model (02NN) does not have a touch screen, but can simulate it via serial commands.

Basic specs:
- Power Voltage: 7.0V ~ 15.0V (Typ.: 12V)
- Operation Current (Typ): 190mA (Backlight on); 60mA (Backlight off)
- Working Temperature (60% RH @ 12V): -10°C ~ 60°C (Typ.: 25°C)
- Working Humidity (25°C): 10% ~ 90% (Typ.: 60%)
- Size: 189.1 x 101.2 x 14.5 mm

Serial port specs [must support direct connection to Arduino UNO (ONLY 5V, or using adapter for 3.3V)]:
- Baudrate:
- - Min.: 3150
- - Max.: 3225600 bps (3.225600 Mbps)
- - Typ.: 115200

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN_front.png)

![img](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Doc/DMG80480Y070_02NN_board.png)

### Important tip: don't watch any tutorial on the internet without reading the [Development Guides](https://www.dwin-global.com/development-guide/).

After reading the development guides, you may find these posts helpful:

- [Basic commands for DGUS II mode](https://github.com/rtek1000/DMG80480Y070_02NN_1st/tree/main/Cmd#readme)

- [Arduino sketch (basic, with CRC)](https://github.com/rtek1000/DWIN_DGUS_HMI/blob/master/examples/Hello_World_crc/Hello_World_crc.ino)

- If the display does not respond to commands from the serial port, see this [page](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Kernel/Readme.md).

------
## Flash memory

Just out of curiosity, I found a W25Q128 memory on this Dwin display (and a T5L0 IC). The data is saved in this memory in virtual partitions of 256kB, so it might be interesting for the ESP32 (or other uC) to program the data from the W25Q128 memory, which is SPI (if the data is not encrypted, which I believe it is not, so as not to reduce performance, I'm still going to see if I can check that). The file number is saved in each virtual partition, for example file 32.* must be saved in position 32x256kB.

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/NOR_Flash_W25Q128.png)

------

## Capacitive touchscreen

Regarding the possibility of installing a capacitive touchscreen on this display model, Dwin said it is not possible. Checking the board we can see that the pins that are dedicated for communication via I2C seem to not have tracks connected to the IC T5L0 pins. And as the pins are very narrow and very close together (Pitch 0.4mm), it must be difficult to adapt wires to this board.

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN/Touchscreen_capacitive.jpg)

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN/Touchscreen_capacitive2.png)

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN/Touchscreen_capacitive1.png)

## Resistive touchscreen

Support for Resistive Touchscreen is on the board, it may be necessary to check the datasheet of the model with the touchscreen (DMG80480Y070_02NR) to see if there is a difference in the programming of the T5LCFG.CFG file.

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN/Touchscreen_resistive.jpg)

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN/Touchscreen_resistive2.jpg)

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN/Touchscreen_resistive1.jpg)
