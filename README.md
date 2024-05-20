# Crux1 Klipper Config

This repo contains a tested working configuration for Klipper for the Tronxy Crux1 printer in it's unmodded, stock form.  
A precompiled binary for the firmware is provided as well but I would recommend you to build your own up to date one from the current [Klipper main branch](https://github.com/Klipper3d/klipper).

The firmware is compatible with both versions of the Tronxy motherboard (STM32F446 and GD32F4XX) since both chips are virtually identical.

---

### Preface

This guide only contains the steps on how to build and flash the firmware for the Tronxy Crux1 printer itself.  
It does not go over the entire Klipper host controller setup as this is simply out of scope of this repository.  
To follow it you must already have a fully set-up and working Klipper installation running on a Raspberry Pi.

If not, head over to the official [Klipper documentation](https://www.klipper3d.org/Installation.html) and follow that.

Continue here once you have a running Klipper host controller with either OctoPrint or Mainsail set-up.  
You will need a web UI for controlling your printer as it's display won't be usable anymore.

### Building the firmware

On your Klipper Raspberry Pi go into the Klipper directory and run `make menuconfig`.  
This will open a dialogue where you have to choose the options for the printer MCU.

Set the options to the following:  
- Micro-controller Architecture: STMicroelectronics STM32
- Processor model: STM32F446 (this applies to the GD32F4XX as well!)
- Bootloader offset: 64KiB bootloader
- Communication interface: Serial on USART1

<img src="https://i.imgur.com/8Z3y4qH.png" alt="buildoptions" width="800"/>

Hit Q and Y to save and close the dialoge.  
Enter `make` in the terminal to start the build process, this will take a moment.

If the build was successful you will end up with the firmware file in `out/klipper.bin`  
Copy this to a place where you can easily access it later.

### Flashing the firmware onto the printer

Before continuing here, please make sure that your printer is running at least bootloader version v1.58.  
You can check that by looking at the top left corner on the printers display when turning it on.  
If your version is older, download the firmware update files from the Tronxy website and flash them on your machine first.

You can find those here:  
https://www.tronxy3d.com/pages/crux-1-open-source-firmware

If you have a newer version you can go ahead with flashing Klipper onto the machine.

Get an SD card (ideally not the included one as it's unreliable) and format it in FAT32.  
Create a new folder on that card called 'update' and copy the klipper.bin file from earlier into it.  
Rename the klipper.bin file to fmw_tronxy.bin

Turn your printer off, plug in the SD card and turn the printer back on.  
The flashing process will start automatically.

<img src="https://i.imgur.com/Zz7qmyl.jpeg" alt="buildoptions" width="800"/>

PLEASE NOTE:  
After flashing Klipper onto your printer the display will become useless and permanently show the white boot screen.  
Unfortunately the display that Tronxy uses is a proprietary design that is not compatible with any other firmware.

### Configuring the printer

Copy the printer configuration file 'crux1_printer.cfg' onto your Klipper Pi and rename it to printer.cfg  
Change the serial connection port according to your system (look into /dev/serial/by-id/ to find the right one).

Save the file and restart the Klipper host process to apply the changes.  
Klipper should now be able to connect to your printer and control it.

Congrats! You Klipperized your Crux1!
