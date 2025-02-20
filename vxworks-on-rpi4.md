
## 1. Build the VxWorks VSB for RPI4
- Open Workbench 4/VxWorks 7 and create a VSB for the RPI4
	- name it something like `rpi4-vsb`
- for the BSP, select `rpi_4_xxxxxx` like this and hit `Finish`:
![[vxworks-on-rpi4-1739894945435.webp|387x489]]

- Build the VSB, you shouldn't get any errors

## 2. Create, Configure and Build the VIP based on the VSB

- In Workbench, create a VxWorks Image Project (VIP)based on the above VSB
	- name it something like `rpi4-vip`
- Choose "Based on a source build project", select the previous `rpi4-vsb` and hit `Finish`
	- We'll customize in the next step
![[vxworks-on-rpi4-1739895265814.webp|389x503]]


### 2.1 Configure the VIP project
- open the VIP project `Kernel Configuration` tool
- Select the `Bundles` Tab in the middle of the screen like this and add the `Standalone kernel Shell` bundle by right-clicking and hitting `Add`:
![[vxworks-on-rpi4-1739895693630.webp|500x322]]
- return to the `Components` Tab and hit `Ctrl-F` to search for `CONSOLE_BAUD_RATE` and make sure it's set to 115200
- Optionally, add components that you want available in the shell 
	- `ping (FOLDER_PING)` enables network ping
	- `ifconfig (INCLUDE_IFCONFIG)` enables setting IP address
	- `routec (INCLUDE_ROUTECMD)` change IP routing
	- `telnet (FOLDER_TELNET)` add `INCLUDE IPTELNETS` to enable telnet server




## 3. Create Boot Device and copy Files

- Format a micro SD card with FAT32 filesystem
- If you've built your own u-boot, add that along with the additional RPI4 boot files to the root directory of the micro SD Card.
	- A pre-built u-boot environment is here on GitHub: https://github.com/rmoorewrs/VxWorksRPI4BootDisk
	- clone the project and unzip the zip file to the micro SD card
```
$ git clone https://github.com/rmoorewrs/VxWorksRPI4BootDisk
$ cd VxWorksRPI4BootDisk
$ unzip vxworks-rpi4-boot-disk.zip
$ sudo cp vxworks-rpi4-boot-disk/* /mnt/location-of-your-sdcard
```

- The root directory should look something like this
```
bcm2711-rpi-4-b.dtb
config.txt
fixup4.dat
rpi-4b.dtb
start4.elf
u-boot.bin
uboot.env
```
The `config.txt` file should look something like this, in order to enable the GPIO UART:
```
# Enable mini UART
enable_uart=1
# Run in 64-bit mode
arm_64bit=1
# Use Das U-Boot
kernel=u-boot.bin
```

## Appendix:  Attach Serial Port to GPIO

- Note that Raspberry Pi GPIO pins are 3.3V
	- Connecting to 5V will damage the board 
- easiest solution is an FTDI USB RS232 cable with GND,TX, RX connections
- Recommended Cable: FTDI chipset USB/232 adapters on Amazon 
  https://www.amazon.com/dp/B07B5TP67V
![[vxworks-on-rpi4-1740068768847.webp|195x325]]


Here is the Raspberry Pi GPIO Layout, we're using the 3 pins near the top right: Pin 6 (Ground), Pin 8 (UART TX) and PIN 10 (UART RX)
![[vxworks-on-rpi4-1740068990218.webp|461x727]]


### FTDI USB/Serial Cable to RPI GPIO Pinout

| Function | GPIO Name | GPIO Pin | Comment                | FTDI Wire Color |
| -------- | --------- | -------- | ---------------------- | --------------- |
| GND      | Ground    | 6        | many other pin options | Black           |
| TX       | GPIO 14   | 8        | UART TX                | White           |
| RX       | GPIO 15   | 10       | UART RX                | Green           |

### Optional: Glue the Single Pin Connectors Together

![[vxworks-on-rpi4-1740071287468.webp|354x277]]

In the photo above, I wrapped cotton thread around the 3 pin connectors and added a drop of superglue. 

***Be careful***: it's easy to ruin the connectors with too much superglue! 

***Tip:*** squirt out a drop of superglue onto a piece of wax paper, silicone mat, etc and then add tiny amounts to the thread with a toothpick.

### Optional: Tie Back the Additional Wires with Heatshrink Tubing

Since you're only using 3 of the connectors on the FTDI/USB serial cable, the extra connectors can get in the way. Rather than cut the extras off (which runs the risk shorting something) I used a piece of heatshrink tubing to hole the extra connectors out of the way. This is completely non-destructive and allows you to potentially use the other signals in the future. 
![[vxworks-on-rpi4-1740071944476.webp|500x362]]


## Additional Notes:
Useful tutorial on building rpi4 toolchain here (but I didn't get this one working):
https://hechao.li/2021/12/20/Boot-Raspberry-Pi-4-Using-uboot-and-Initramfs/

This tutorial was actually better, and I got u-boot up and running:
https://danmc.net/posts/raspberry-pi-4-b-u-boot/


### Manually Loading the uVxWorks image in u-boot:
``
```
fatload mmc 0:1 0x100000  uVxWorks
```

