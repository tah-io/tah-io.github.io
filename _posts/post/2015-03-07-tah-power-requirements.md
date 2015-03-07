---
layout: post
title: "Tah power requirements"
modified:
categories: post
author: dhiraj_jadhao
excerpt:
tags: []
image:
credit:
date: 2015-03-07T08:00:00
comments: true
share: true
---

![tah pinout diagram-min](https://cloud.githubusercontent.com/assets/2573556/6540338/acca79fa-c4be-11e4-9fff-45682682c949.png)

The operating voltage for Tah is 5 Volts. The Tah board comes with a USB port etched on the PCB itself; this port can be used for powering Tah by plugging it into your computer or a 5V AC-DC usb power adapter. The Tah board can also be powered with a 5V regulated battery by using the VIN and GND pins on board. Do not operate the board over 5 volts, doing so will possibly damage it.

Tah provides a maximum of 40mA on each I/O pin. Onboard, It has a 3.3V regulator which can provide a total of 50mA DC current, of which 8.5mA is used to power the BLE module and the rest is available on the 3.3V power pin.

The operating voltage for the Tah BLE module is 3.3V which is supplied by the onboard regulator. The Tah BLE module draws 8.5mA of current when in active mode and requires just 400uA - 1.5mA while in sleep mode.

Following are the power pins given on the Tah board:

— VIN: This pin can be used if you want to externally power the Tah board. Never use VIN and USB power at the same time, only use one type
of power input at any given time. If you have powered the Tah using an onboard usb then this pin can be used as a 5V output.

— 5V: The regulated power supply used to power the microcontroller and other components on the board. This can come either from VIN or be
 supplied by USB or another regulated 5V supply.

— 3.3V: A 3.3 volt supply is generated by the on-board regulator. Maximum current drawn is 50 mA.

— GND: The Tah has 2 onboard ground pins.