---
layout: post
title: "Tah and Shields Testing"
modified:
categories: post
author: vikas_gaikwad
excerpt:
tags: []
image:
credit:
date: 2015-02-09 22:15:00 -0800
comments: true
share: true
---

Tah and Shields Testing 
===

As we all knows we had successfully completed the Tah on crowdfunding campagn, with lot of love and appreciations from the backers we are fully energized and focusing towards for successful shipping of Tah boards and all Tah Shields, to delevered a quality based products by having a regerous testing of boards and shields. To test all the boards i working on automisation of all the Tah boards and shields, all the Tah boards go through the automatic testing with the help of **Tah Testbed** the complete hardware is designed based on Raspberry pi and similarly 2 Shields need to go through the manual testing and IR shiedlds will go through the semi-automatic testing and all the logs are made automatic using the raspberry pi.

While i was working on Tah Testbed design it was half way cooked design but i found there are the long way to go in the design of Tah Testbed, previously the Ali and Anuj was working on this Tah Testbed, Ali had design the Testbed on general purpose pcb with the mess of wires, when i saw that pcb i know, i better not to go through that pcb it was very difficult for me to decode what the actual circuit is.
During that period i came accross the **Git Hub** it was first time were i was interacting with git hub were all the source code and schematic files were stored, initially it was hard for me to interacting with git hub. Once i got an idea  of the first version of Tah Testbed hardware, I planed to designed a new home made pcb for Tah Testbed, by simply having few modifications to hardware design i finallize the circuit and i did the Schematic and layout design in Eagle CAD software which mainly used for professional PCB design.

I designed the complete schematic and pcb layout for first batch of Tah having different footprint.The complete technical descriptions of the Tah Testbed is as below.

**Hardware:**

1.Raspberry Pi B or B+  
2.16:1 Mux 74HCF4051  
3.8:1 Mux HCF4051  
4.BC547  
5.26pin FRC cable  
6.5v Buzzer  
7.16x2 LCD  
8.Multistand wires  
9.Push Buttons  
10.Resistors  
11.AVR ISPmkII programmer  

You will find the exact value components in the schematic file, I would explain the basic concepts behind the Tah Testbed based on raspberry pi, Here i had design the Testbed with some descrete and few integrated circuits chips and use the LCD display to show testing messages while conducting actual testing. Here we had to test GPIO pins and Analog pins of Tah boards this we did it completely automatic with raspberry pi acting as a central processing unit. Tah is an arduino compatible board so to test Tah over Rpi need to upload the Tah test sketches from raspberry pi without using arduino IDE their is method of doing this with [Arduino Makefile](http://www.linuxcircle.com/2013/05/15/programming-and-uploading-arduino-sketch-without-ide/) i followed this tutorial for doing this.

When i was ready with my first testbed design their was a test run of testing 120 Tah boards on testbed and need to simulate as per the final test process for 800 Tah boards, through this we were going to get familier with what what problems would come while doing actual testing of boards. Before starting the actual testing on testbed the test flow would be like this, usually test was divided in 2 steps first was **COLD Test** and second was **HOT Test**, for thouse who are not aware of this terms, no worry this means in COLD Test : The test needs to go through the visual inspections without giving power to citcuit, under this we look for soldering joints, any loose connections, polarity of polar components etc, and **HOT Test** done by power ON circuit and perform some measurements or perform automatic test overthe testbench.

Before moving to testing on Testbed we need to upload the Tah Bootloader to the ATmega32u4 chip on Tah board to became as a Tah, while i was working on this i plan to do the bootloading of the Tah boards on testbed itself so test it on Raspberry pi and succesfully done the bootloading using raspberry pi if any one wants to use their raspberry pi as AVR programmer the can [Tah Bootloader](http://docs.tah.io/examples/tah-bootloader/) tutorial this works and also that method was a way faster than the using Arduino as a ISP, but the major problem i realise while programming Tah, that SPI pins of Rpi works on 3.3 V logic and microcontroller on Tah works on 5V TTL logic level i tried it on 3.3V level buit found it did not work it contineously resets the Tah board.  I descided to go for seperate ICSP programmer for uploading the bootloader  i used AVR ISPmkII clone as In System Programmer and found this works faster than Raspberry pi so finally kept this beacuse it reduces my time for bootloading the boards, only disadvantage of this was it supports on Windows OS only with avrdude.
![tahbootloading](https://cloud.githubusercontent.com/assets/8414343/6124556/792ab990-b134-11e4-8b58-f81bc3b00e59.jpg)

  
Now I was set for doing the testing of 120 Tahs, Me and Anuj startted the testing on both Testbed we realise the thir needs a improvements in the Software part of Raspberry pi we were running the python scripts for uploading the Tah Test sketches to Tah from Raspberry pi over USB we face the problems like USB ports confilts(change)during runtime and after every restart of testing we were lossing the previous records of logs in CSV files, log gets overwritten everytime,because of the changein USB port for most of the time the arduino sketch gets recompile due to this testing time was increasing from 50 sec to 3 mins.Finally we done testing of 120 boards in contineous of 6 hours testing and found  8 Tah boards faulty with BLE problems.

![oldtestbed](https://cloud.githubusercontent.com/assets/8414343/6124982/f19b4a40-b137-11e4-931b-451fab090100.jpg)
 
After completing testing i did debuging of those 8 Tah boars and found problems in BLE modules i replaced them with new ones and they did work well .


After this test run of Tah Testing we divided the kind of work like software upgradations and Hardware design for final Tah testbed with new footprint i was mainly working on development of 4 new Tah Testbeds, one thing was confirms through that test run that their was no problems in hardware  of testbed so we only need to upgrade the software. I did change the footprint and added new to testbed and finalise and completed the development of 4 new testbeds and Anuj had done the software upgradations so in this testing flow was remain the same as follows

* Tah board goes through COLD Test
* Bootloader uploading using AVR ISPmkII
* Now HOT test on actual Testbed by pressing Start key on testbed.
* On Raspberry pi it will actually upload the test sketches to Tah and test each individual pin sets for Logic High or not using MUX logic   	and status will be displayed on LCD display.
* On completins it will upload the ArdSCL sketch to board and logs result to CSV file.

You will find all the Tah Testbed source files [here](https://github.com/RevealingHourCreations/Tah_Testbed)

![testbedcompare](https://cloud.githubusercontent.com/assets/8414343/6125009/0c270d36-b138-11e4-8347-c29e112403fc.jpg)


Next task was testing the 30 new Tah boards on new design of Tah Testbed by following the above test procedure and found that it had improved as expected here one Tah board took 50 sec time for doing actual testing on testbed definitly it had improved from 3 mins to 50 secs. Sucessfully completed the 30 Tah boards testing with good quality test, during this run we found 3 Tah boards faulty while bootloading, as per our assupmtions due to improper soldering of crystal or Atmega32u4 microcontroller chip.

![tahtesting](https://cloud.githubusercontent.com/assets/8414343/6125025/26b91518-b138-11e4-88a7-6f5cafb4cbba.jpg)

The new batch of assembled Tah boards came from Thuse Electronics, our new and very important task was to complete the Tah boards testing, we had completed the bootloading of all Tah boards using AVRISPmkII programmer, On bootloading we found 4 Tah boards not able to burn bootloader.On next day i started the actual testing of Tah boards on Tah Testbed the rate of testing a Tah boards had quite improved. I completed the testing of 480 boards , yes while testing those boards i had few problems in arduino sketch uploading, problems like upload failed  and programmer not responding etc, in all these testing found 24 boards faulty and able to recovered 10 Tah boards in next round of testing and now all the boards had tested sucessfully.

**Shields Testing**

Tah comes with 3 different types of shields for different applications and it really helps the developers to explore the basic things in the electronics. Similarly we have a demands for shields the shields like IR remote shield, Shield-Shield, Sensor Relay shield i will give you the basic introductions about all these shields and it applications the IR shield is develope for replacing the conventional IR remotes with smart phone and make one universal remote.Shield-Shield is the unique shield of Tah, as we all know Tah is arduino compatible so keep that feature and compatibility of arduino shields with Tah this shield is design, the third shield is the Sensor Relay shield this helps to interface with different types of sensors and to control AC mains home appliances with Tah this can be usefull for small home automation projects.

Now lets talk more about the testing of all these shields, similarly here i am following the same approach of testing COLD and HOT test only the test points were different and this was going to be mostly the mannual testing.To log the testing data and to avoid mannual report generation of testing i had designed the one switch boards with raspberry pi to log the test data while testing by just switching we would able to generate the test report in the CSV file format.

![testswitch](https://cloud.githubusercontent.com/assets/8414343/6125056/5b0828d6-b138-11e4-983f-0763977a0d30.jpg)

Testing of Sensor Relay shield in this shield i had two important parameters to test like voltage regulator testing and realy testing.we were testing voltage regulator output by DMM directly but for testing relays using Tah with relay switching sketch on power on it will start toggling the relays when tested completely the data gets log into raspberry pi using switchboard.

![sensorrelay](https://cloud.githubusercontent.com/assets/8414343/6125090/b559b4b2-b138-11e4-989d-a6d787ef087c.jpg)


Now test process for testing Shield-Shield this will involved the complete manual testing in this we had only one test point that of voltage regulator output measurement using DMM and we done with testing, all the COLD test were done successfully.

![shield-shield](https://cloud.githubusercontent.com/assets/8414343/6125093/cab4b0a0-b138-11e4-989a-4e5343d600d0.jpg)

Now the IR Remote Shield, this involves some automisation and this shield had to go from regourous testing because it had a two very important parts to test IR transmitter and IR receiver section this will done using Tah which runs the test sketch of IR testing contineously based on interrupt mechanism, lets talk about COLD test in this we need to verify the polarity of all the IR transmitter LEDs and we procced for doing HOT test using Tah and Logitech web camera. Under camera we observed the IR transmitter leds and to test IR receiver we were using any external remote when there were a interrupt comes from external remote the LED on Tah starts blinking or it remains litups

![irtest](https://cloud.githubusercontent.com/assets/8414343/6125107/e757c990-b138-11e4-9118-6006f9cc0b8d.jpg)

This is how the testing of all the boards and shields going to be, when we had all the productions done successfully.


 









