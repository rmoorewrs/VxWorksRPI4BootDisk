This zip file contains all the bootloader files required to boot VxWorks on a Raspberry Pi 4, except for the the uVxWorks kernel. You have to build and add the uVxWorks kernel yourself. 

Steps to create a bootable micro SD card to boot VxWorks on RPI4

1) Format an SD card with FAT32 filesystem
2) Mount the SD card on your development machine (Windows, Linux, MAC, etc)
3) Unzip the contents of the ZIP file
4) Copy the contents of the ZIP file into the root directory of the SD card (all files in the top/root directory, no folders). 
	- At this point, the SD card will boot to a u-boot prompt, but it won't be possible to boot VxWorks
5) Configure and Build your RPI4 VxWorks Image Project (VIP), then copy the uVxWorks kernel file to the root of the SD card. 
	- now the SD card should boot to a VxWorks prompt
