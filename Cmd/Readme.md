Basic commands (from file  [T5L_DGUSII Application Development Guide V2.8.pdf](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Doc/T5L_DGUSII-Application-Development-Guide-V2.8-0225.pdf)) :

##### Read GUI-status:
If sending commands at intervals shorter than 20ms: Need to read GUI state before sending new order CMD

- CMD: 5A A5 04 83 0015 01
- - RET: 5A A5 06 83 00 14 01 0000 (0x0000=free)
- - RET: 5A A5 06 83 00 14 01 0001 (0x0001=processing)

  
##### Load icon:
- CMD: 5A A5 0F 82 5440 3007 0001 0168 0168 0001 FF00

> 7.22.9 Icon Display
> Displays icon No. 01 in the No. 48 icon library.
> 5A A5 0F 82 5440 3007 0001 0168 0168 0001 FF00
> 0x5AA5: Frame header;
> 0x0F: Data length;
> 0x82: Write instruction;
> 0x5440: VP;
> 0x30: Icon library No.48;
> 0x07: 07 icon Write instruction;
> 0x0001: display an icon;
> 0x0168 0x0168: The starting display coordinates of the upper left corner of the icon (360, 360);
> 0x0001: Icon No. 1 in the No. 48 icon library.
> 0xFF00: Terminator


##### Touchscreen Simulated:
- CMD: 5AA5 0B 82 00D4 5AA5 0004 00EE 008F

> 0xD4: 0x5AA5= enable the operation once, clear after operation.
> 0xD5: press mode. 0x0001=press, 0x0002=release, 0x0003=keep pressing, 0x0004=touch (press + release)
> 0xD6: X coordinate of press position.
> 0xD7: Y coordinate of press position.
>
> After simulating mode 0x0001 and 0x0003, must simulate 0x0002.


##### Load BackGround (BG):
- CMD: 5A A5 07 82 0084 5A01 0001


##### Read Display current BG page ID. (Read only):
- CMD: 5A A5 04 83 0014 01
- - RET: 5A A5 06 83 00 14 01 0007 (0007 is page 07)


##### Backlight max:
- CMD: 5A A5 07 82 0082 0064 0000

##### Backlight OFF:
- CMD: 5A A5 07 82 0082 0000 0000

##### Backlight mid:
- CMD: 5A A5 07 82 0082 6432 03E8


##### Read Backlight value:
- CMD: 5A A5 04 83 0031 01
- - RET: 5A A5 06 83 00 31 01 5A 32 (32= 50%; 64=100%)


##### Play Buzzer 1s (The buzzer sounds for 1 second, 1000ms/8ms=125=007Dh):
- CMD: 5A A5 05 82 00A0 007D


##### Play Buzzer 0.5s (The buzzer sounds for 0.5 seconds, 500ms/8ms=62.5=003Eh):
- CMD: 5A A5 05 82 00A0 003E
