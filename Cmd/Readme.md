# Basic commands
(from file  [T5L_DGUSII Application Development Guide V2.8.pdf](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Doc/T5L_DGUSII-Application-Development-Guide-V2.8-0225.pdf)) :
- It has several other functions that could perhaps be better utilized with a display that has its own touchscreen.
- It also has several other interesting commands for drawing, including spectrum graphics and trend graph.

------

The library for Arduino implements commands with CRC, see [here](https://github.com/rtek1000/DWIN_DGUS_HMI/).

------

### Note 1 - timing:
Programs made in DGUS II, that use timing, showed an interval about 2x faster in the preview simulator than in the real display.
- For example, animating an icon with two images in a loop every 0.5s, the real display did it in 1s.

------

### Note 2 - text:
A thing I wasn't very successful at was writing text.
- I couldn't get the text to be centered.
- The appearance of the letters wasn't very good.
- - Automatic reduction of spaces between characters doesn't seem to work, or doesn't have it.
- - I preferred to edit an image in Paint with text and display it as an icon.
- - - And there are several open sources fonts on the internet.
- There may also be a lack of encoding compatibility, and the characters will not be displayed correctly.
- - For example, when selecting the [UNICODE encoding](https://en.wikipedia.org/wiki/List_of_Unicode_characters) type, the microcontroller must send 2 bytes to write each character on the display.
- - If the program only uses basic (Western) characters 0x00~0x7F, try using [GBK encoding](https://en.wikipedia.org/wiki/GBK_(character_encoding)), and you can only send 1 byte to write each character.
- ![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/Font%20Generator/Encoding_type.PNG)

- If the letter appears wrong (unrecognizable):
- - Try generating the font with the program '[Font generator](https://www.dwin-global.com/tool/)' (字库生成器.exe version 2.0.0.1).
- - In Dot Matrix Size, leave the same value for X and Y.
- - Be careful with the use of 'Scale' and 'Shift'; View all characters so that none are cut off.
- ![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Doc/Font%20Generator/Font%20Generator.PNG)

------

### Note 3 - CRC:
The commands below do not yet have the CRC, after adding the CRC, byte 2 (Data length) has its value increased by 2, and the CRC has 2 bytes (2x 8-bits).
- Example CMD without CRC: 
> 5A A5 04 83 00 15 01

- Example CMD with CRC: 
> 5A A5 06 83 00 15 01 E6 F0

The DGUS II software has an appropriate terminal (SP Order) for sending orders (commands) and receiving responses from the display. The terminal is in a tab along with the CFG file generator (CFG Edit), in menu 'Settings' - 'DGUS serial port' - 'SP Order'. And the SP Order terminal already has the option to calculate and send commands with CRC

------

### Note 4 - response:
In general, commands sent to the display are responded to, this can be used to check whether the Order (command) was received by the display.
- Example for command (CMD): 5A A5 04 83 0015 01
- - Response (RET): 5A A5 06 83 00 14 01 0000
- Example for command: 5A A5 05 82 00A0 007D
- - Response: 5A A5 03 82 4F 4B

#### No need to send CR (\r) or LF (\n) line terminator

------

### Note 5 - image:
The image can be edited in any editor, but I had an Android app project made in [B4X](https://www.b4x.com/), and I preferred to positioning the background images and icon positioning using B4X, which has zoom in the editor.
- Unfortunately using an Android tablet as an HMI didn't work very well, it happened to burn 2 PCB boards, apparently the models I used (Rk3028 chip) are not devices for 24 hour use. Others with a similar chip model, but without BGA soldering, are still working, but I preferred to avoid tablets for this application, although Android apps are much better than HMI displays.

------
------

## Commands:

### Read GUI_status feedback:
If sending commands at intervals shorter than 20ms: Need to read GUI state before sending new order CMD

- CMD: 5A A5 04 83 0015 01
- - RET: 5A A5 06 83 00 14 01 0000 (0x0000=free)
- - RET: 5A A5 06 83 00 14 01 0001 (0x0001=processing)

> The operation state feedback is in millisecond unit level, which is generally applied in special cases. The user can judge whether the GUI kernel is occupied by the DWIN - OS program.

------
![img](https://raw.githubusercontent.com/rtek1000/DMG80480Y070_02NN_1st/main/Cmd/Icon%20Display%20cmd.png)

### Load icon:
- CMD: 5A A5 0F 82 5440 3007 0001 0168 0168 0001 FF00

> 7.22.9 Icon Display
> - Displays icon No. 01 in the No. 48 icon library.
> - 5A A5 0F 82 5440 3007 0001 0168 0168 0001 FF00
> - 0x5AA5: Frame header;
> - 0x0F: Data length;
> - 0x82: Write instruction;
> - 0x5440: VP; -----> VP of the "drawing board" ('Basic Graphic')
> - 0x30: Icon library No.48;
> - 0x07: 07 icon Write instruction;
> - 0x0001: display an icon;
> - 0x0168 0x0168: The starting display coordinates of the upper left corner of the icon (360, 360);
> - 0x0001: Icon No. 1 in the No. 48 icon library.
> - 0xFF00: Terminator

- Only one icon is loaded on each VP "drawing board".
- To remove the icon, a command to display an icon ID that does not exist in the library (file 48.ICL) can be sent.
- When using a VP address for each 'Basic Drawing', even if the Background image is changed, the icon will remain as it was, until the VP value is modified.
- - When returning to the background, the icon will also be reloaded.

Note1: There may be a BUG in the kernel that prevents several icons from being displayed at the same time, with one icon for each 'Basic Graphic' and each one with a different VP address. I tried to create a digital clock, but each time a digit was updated, the previous one was removed. The solution may be to use Icone Var_Icon which loads the icon according to the value placed in the VP address. The problem may be with the amount of VP that the Icon Display command is writing. Each VP has 1 word (16-bit), but the Icon Display command needs 6 words (6x 16-bit), so you have to reserve the appropriate amount of unused VP, another item cannot share these VP.

Note 2:
- To load an icon, the program must have a 'Basic Graphic' item on the background image, which will have a VP address.
> 7.22 Basic Graphics (menu 'Graph Show' - 'Basic Graphic')
> - The basic graphics control is to define a "drawing board" ('Basic Graphic' item) function in the display configuration file 14.BIN, and the specific drawing operation is determined by the content of the variable memory pointed to by * VP. Users can realize different drawing functions by changing the data in the variable memory.

Note 3:
- The DGUS II software has an 'Export' button (*.xls file) that can be used as a list of VP addresses in use, including the background image and type of item used (unfortunately it is not yet in ascending VP order).

Note 4:
- Animation Icon: The manual (v2.8) mentions a limit of 255 (0xFF) for the image identification number for use with animated icons. This way, numbers above 255 cannot be used to animate icons (ICON_Stop/ICON_Start/ICON_End parameters). But for Var Icon it is possible to use file identification numbers above 255.

------

### Touchscreen Simulated:
- CMD: 5AA5 0B 82 00D4 5AA5 0004 00EE 008F

> 0xD4: 0x5AA5= enable the operation once, clear after operation.
> 0xD5: press mode. 0x0001=press, 0x0002=release, 0x0003=keep pressing, 0x0004=touch (press + release)
> 0xD6: X coordinate of press position.
> 0xD7: Y coordinate of press position.
>
> After simulating mode 0x0001 and 0x0003, must simulate 0x0002.

------

### Load Background - PIC_ Set:
- CMD: 5A A5 07 82 0084 5A01 0001


##### Read Display current Background page ID. (Read only):
- CMD: 5A A5 04 83 0014 01
- - RET: 5A A5 06 83 00 14 01 0007 (0007 is page 07)

------

### Read Backlight value:
- CMD: 5A A5 04 83 0031 01
- - RET: 5A A5 06 83 00 31 01 5A 32 (32= 50%; 64=100%)

##### Backlight max (100%):
- CMD: 5A A5 07 82 0082 0064 0000

##### Backlight mid (50%):
- CMD: 5A A5 07 82 0082 0032 0000

##### Backlight OFF (0%):
- CMD: 5A A5 07 82 0082 0000 0000

##### Backlight mid and timer:
(10 seconds after the screen backlight automatically reduced to 0x32 brightness)
- CMD: 5A A5 07 82 0082 6432 03E8

------

### Play Buzzer 1s (The buzzer sounds for 1 second, 1000ms/8ms=125=007Dh):
- CMD: 5A A5 05 82 00A0 007D

##### Play Buzzer 0.5s (The buzzer sounds for 0.5 seconds, 500ms/8ms=62.5=003Eh):
- CMD: 5A A5 05 82 00A0 003E

Note: The Buzzer frequency can be modified in the CFG file settings, I don't know if it is possible to modify it during running time
- Ref.: 0x27 to 0x2D
> - Buzzer frequency= BUZZ_Freq_DIV1) KHz.825753. 6
> - Factory settings: DIV1=0 x6E, DIV2=0 x0 BB8, correspond to the 2. 5 KHz frequency.
> - Buzzer duty ratio settings: High level duty ratio=BUZZ_Freq_Duty/ BUZZ_Freq_DIV2. Factory settings: 0x00F0 corresponds to 8% high level duty ratio.
> - Buzzing time after touch, unit 10 ms; Factory setting: 0x0A
