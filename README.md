# Spark STM32 Bootloader

After building with Eclipse or with `make` in the build folder, use JTAG to flash the .bin or .hex file to address 0x08000000 on the STM32.
The device should appear as "STM Device in DFU Mode" on the Host platform.

**NOTE: following instructions are for Core hardware v0.1 (delivered to beta testers June 2013)
  and are not necessarily true for later versions of the Core.**

## Use of LEDs and BUTTON

* Both LED1 and LED2 should toggle simultaneously for 250ms indicating that USB firmware update mode has been entered.
* Press BUT just once to exit USB firmware update mode and enter User Application mode. If no application project is loaded, USB firmware update mode is entered by default.
* Press BUT for > 1sec during reset to enter USB firmware update mode.

## Flash Code via USB

Install open-source "dfu-util" on Mac (brew install dfu-util), Windows, or Linux, downloadable from:
http://dfu-util.gnumonks.org/index.html

Add "dfu-util" related bin files to PATH environment variable

List the currently attached DFU capable USB devices by running the following command on host:

    dfu-util -l

The Core, Debug or H103 boards in DFU mode should be listed as follows:

    Found DFU: [1d50:607f] devnum=0, cfg=1, intf=0, alt=0, name="@Internal Flash  /0x08000000/12*001Ka,116*001Kg"

If the macro SPARK\_SFLASH\_ENABLE is uncommented in platform\_config.h, then the External Serial Flash should also be listed as follows:

    Found DFU: [1d50:607f] devnum=0, cfg=1, intf=0, alt=1, name="@SPI Flash : SST25x/0x00000000/512*04Kg"

For Flashing core-firmware.bin using the bootloader,
cd to the core-firmware/build folder and type the below command to program the core-firmware application to Internal Flash starting from address 0x08005000:

    dfu-util -d 1d50:607f -a 0 -s 0x08005000:leave -D core-firmware.bin

For flashing factory core-firmware application to External Flash starting from address 0x00020000, run the following command:

    dfu-util -d 1d50:607f -a 1 -s 0x00020000 -D core-firmware.bin

To build the project using command line option, cd to the "build" folder and run "make clean dependents all".
