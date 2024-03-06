# DWIN DMG80480Y070_02NN
First impressions of the Dwin display model DMG80480Y070_02NN and Basic commands

If you are looking for a low-cost display and are targeting an SSD1963, it might be interesting to check out Dwin displays.

This model DMG80480Y070_02NN is 800x480 in 7 inches, with only additional Buzzer and communication connector.

- Communication via serial 8N1 (TTL(default)/RS232 selectable via jumper)
- Can make the buzzer beep
- Can control Backlight
- Has CRC16 error control (Modbus style with swapped bytes)
- This model (02NN) does not have a touch screen, but can simulate it via serial commands.

![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/DMG80480Y070_02NN_front.png)

![img](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Doc/DMG80480Y070_02NN_board.png)

### Important tip: don't watch any tutorial on the internet without reading the [Development Guides](https://www.dwin-global.com/development-guide/).

After reading the development guides, you may find these posts helpful:

- [Basic commands for DGUS II mode](https://github.com/rtek1000/DMG80480Y070_02NN_1st/tree/main/Cmd#readme)

- [Arduino sketch (basic, with CRC)](https://github.com/rtek1000/DWIN_DGUS_HMI/blob/master/examples/Hello_World_crc/Hello_World_crc.ino)

- If the display does not respond to commands from the serial port, see this [page](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Kernel/Readme.md).
