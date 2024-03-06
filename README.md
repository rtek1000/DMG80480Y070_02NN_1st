# DMG80480Y070_02NN_1st
First impressions of the Dwin display model DMG80480Y070_02NN and Basic commands

If you are looking for a low-cost display and are targeting an SSD1963, it might be interesting to check out Dwin displays.

This model DMG80480Y070_02NN is 800x480 in 7 inches, with only additional Buzzer and communication connector.

- Communication via serial 8N1 (TTL(default)/RS232 selectable via jumper)
- Can make the buzzer beep
- Can control Backlight
- Has CRC16 error control (Modbus style with swapped bytes)

Important tip: don't watch any tutorial on the internet without reading the [Development Guides](https://www.dwin-global.com/development-guide/).

- [Basic commands for DGUS II mode](https://github.com/rtek1000/DMG80480Y070_02NN_1st/tree/main/Cmd#readme)

- If the display does not respond to commands from the serial port, but loads the first image, the display may be in TA mode, see this page: [kernel upgrade](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Kernel/Readme.md)https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Kernel/Readme.md
