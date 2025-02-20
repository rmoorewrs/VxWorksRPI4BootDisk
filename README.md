# VxWorksRPI4BootDisk
This zip file contains all the bootloader files required to boot VxWorks on a Raspberry Pi 4, except for the the uVxWorks kernel. You have to build and add the uVxWorks kernel yourself. 

## Steps to create a bootable micro SD card to boot VxWorks on RPI4

### 1. Format an SD card with FAT32 filesystem

### 2. Mount the SD card on your development machine (Windows, Linux, MAC, etc)

### 3. Unzip the contents of the ZIP file

### 4. Copy the contents of the ZIP file into the root directory of the SD card
	-  Note: all files in the top/root directory, no folders 
	- At this point, the SD card will boot to a u-boot prompt
	- VxWorks won't boot until you add a uVxWorks kernel to the top level directory

### 5. Configure and Build your RPI4 VxWorks Image Project (VIP)
	- copy the uVxWorks kernel file to the root of the SD card 
	- replace in the RPI4 and boot
	- SD card should now boot to a VxWorks prompt
	- if you've configured the serial port, set 115200,n,8,1 no flow control

