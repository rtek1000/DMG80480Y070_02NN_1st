The display came to me in TA mode, this appears on the SD card blue screen.

- In this TA mode, the display does not work with programs made with the DGUS II software.
- Serial commands for DGUS mode are not recognized.

To switch from TA mode to DGUS mode, you need to change the kernel.

- To change the kernel, you must place the [3 files](https://github.com/rtek1000/DMG80480Y070_02NN_1st/blob/main/Kernel/DGUSII%20Kernel.zip) in the DWIN_SET folder together with the files from a project created in the DGUS II software.
- - The project must have the T5LCFG.CFG file to start the basic display settings in DGUS mode.
 
Note 1: to return to TA mode it is necessary to contact Dwin, as the employee who helped me (from TA to DGUS mode) only gave me the [kernel upgrade](https://www.dwin-global.com/kernel-upgrade/) download link, when I asked about the files to go back to TA mode.

Note 2: if the display has a problem during the file update and stops responding, it may be necessary to [recover the display using a JTAG model PGT05](https://www.youtube.com/watch?v=K-7AfmHDhac).
