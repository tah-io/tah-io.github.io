---
layout: post
title: "Tah VID PID"
modified:
categories: post
author: vikas_gaikwad
excerpt:
tags: []
image:
credit:
date: 2015-02-26 16:12:00 -0800
comments: true
share: true
---

Tah is a Bluetooth Low Energy Arduino compatilble microcontroller based development platform. The Tah has a Arduino Leonardo at its heart, Tah uses the built-in USB of the atmega32u4 rather than a discrete USB-to-serial IC. All USB devices have two identification numbers: a vendor ID (VID) and a product ID (PID). Each organization that produces USB devices pays the USB-IF for a VID, giving them the ability to self-issue PIDs. We've obtained a PID and a pair of VID to compile the bootloader. Lets generate a new .hex file for Tah board with new VID/PID.

Here I had used the Linux machine to compile the new bootloader file, similarly you can do it on Windows machine. Lets start with this tutorial.

## Step 1:
Make sure the [Arduino IDE](http://arduino.cc/en/Main/OldSoftwareReleases) is installed. This ensures you have all the required tools as well as the source code.

## Step 2:

Download [LUFA-111009](http://fourwalledcubicle.com/blog/2011/10/lufa-111009-released/) and unpack it in the same directory as your caterina folder. By default on Ubuntu, this will be **/usr/share/arduino/hardware/arduino/bootloaders**

## Step 3:

This step is optional. Open Descriptors.c located at /usr/share/arduino/hardware/arduino/bootloaders/caterina/Descriptors.c. Navigate down to line 192 (**const USB_Descriptor_String_t ProductString**). Here you can add a product ID string for your PID. For example, I added shown below:
![pid](https://cloud.githubusercontent.com/assets/8414343/6346573/244156c4-bc37-11e4-9ba3-5dac21abee2b.png)

Notice line 194 which specifies a string length of 16 characters. If you entry is less, add some spaces on the end (as done with the Arduino Micro). If it is more, you will need to increase this value in one of two ways. Either increase it here at this line and add spaces to pad out all the existing Arduino strings, or move this line within the #if #endif block.
Do the same for VID, here the entire block for the VID copied from my file:

![vid](https://cloud.githubusercontent.com/assets/8414343/6346767/9df38824-bc38-11e4-9e3d-aa34d5b61b75.png)

## Step 4:

Now open the Makefile(/usr/share/arduion/hardware/arduino/bootloaders/caterina/Makefile) and look for the variable called “LUFA_PATH”. Modifiy this line to point the folder **LUFA-111009** that you have extrated 

## Step 5:

At the beginning of the Makefile were the VID and PID were defined comment out other VID and PID and add the VID and PID of your device.
I have mentioned the VID and PID of Tah as below:


![vid_pid](https://cloud.githubusercontent.com/assets/8414343/6347415/fcc09216-bc3d-11e4-99aa-50460687c76d.png)

## Step 6:

Open your terminal and change directory to /usr/share/arduino/hardware/arduino/bootloaders/caterina and run **sudo make clean** and then **sudo make all** new compiled bootloader file with new VID/PID gets generated is named as Caterina.hex.

You could rename the Caterina.hex to anything you like, I renamed it to **Tah.hex**.

## Step 7:

Lets create a new board file of your board open boards.txt file(located at **/usr/share/arduino/hardware/arduino/boards.txt**). Copy and paste the entry for the Leonardo at the bottom and replace the board name Leonardo with your board, here it is replaced with **Tah** and also replaced the new VID and PID. Here in the boards.txt I've used the second PID for uploading sketches to Tah board.

The entries in the boards.txt file as follows:

![tahboard](https://cloud.githubusercontent.com/assets/8414343/6347820/d57a5b76-bc40-11e4-9bf5-b403bedc672b.png)

Once done restart the Arduino IDE and go to **Tools >> Board>> Tah**, here you will see new entry of Tah board gets added.

## Step 8:

Now we are ready to burn new **Tah.hex** bootloader file to Tah board. I programmed the Tah.hex with external AVR-ISPmkII programmer over the ICSP programming header pins. You could use any other programmer or you could use Arduino as an ISP to burn the new bootloader file.

## Step 9:
We have a new derivative of Arduino Leonardo called **Tah** with our own unique VID and PID for this device. 
