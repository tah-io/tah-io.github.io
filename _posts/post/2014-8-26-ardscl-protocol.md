---
layout: post
title: "ArdSCL Protocol Explained"
modified:
categories: post
author: dhiraj_jadhao
excerpt:
tags: []
image:
  credit:
date: 2014-09-26T08:00:00
comments: true
share: true
---

ArdSCL is basically a protocol which the Tah App uses to control the the Tah board. Below is the description of Tah protocol and how it is used to control the Tah board.

ArdSCL protocol is a string which contains 3 comma separated integers and an end line character “R” i.e, `X,X,XR` - where “X” is an integer value and “R” is the end line character.

**1st Integer:**

The 1st integer in the ArdSCL protocol represents a Pin Type. This value determines how you intend to use the GPIO pins. You can also request the status of all available analog and digital pins. This integer can have a value from `0 to 4`. The assigned relation for the value of the 1st integer is as follows:

*	0 = Digital
*	1 = Analog
*	2 = Servo
*	3 = Request for Analog Pin Status
*	4 = Request for Digital Pin Status

**2nd Integer:**

The 2nd integer in the ArdSCL protocol represents a Pin Number. This integer can take a value from `2 to 13` that corresponds to digital I/O pins on Tah or from `410 to 415` which correspond to `A0 to A5` pins on the Tah respectively. So, if `0` is assigned to the to 1st integer and `13` tp the 2nd integer, then this is equivalent to `pinMode(13,OUTPUT);` in arduino or if your 1st integer is `1` and the 2nd is `410` then it’s equivalent to `pinMode(A0,INOUT);` in arduino.

**3rd Integer:**

The 3rd integer in the ArdSCL protocol represents a Pin Value. This integer dictates what value is to be written on defined pin type. This integer can take different values depending on what pin type has been defined. So, if you have defined the 1st integer i.e Pin Type as `0`, then this integer can take either take up the value `0 or 1`. If you define the 1st integer as `2` which is the Servo function, then this can take up values from `0 to 179`.
  
If you want to turn on the green Led on the Tah board which is connected to pin number 13, then the corresponding AdSCL protocol will be `10,13,1R`. When you toggle the `pin 13` switch in the Tah smartphone app then smartphone send this string to Tah. Once it is received by Tah, it is broken down as follows:

		1st: 0
		2nd: 13
		3rd: 1
   
This is then translated to:   
  
{% highlight ruby %}  
pinMode(13, OUTPUT);
digitalWrite(13,HIGH);
{% endhighlight %}
  
by the ArdSCL sketch.

When you define the 1st integer of ArdSCL string as `3 or 4` then the value of the 2nd and 3rd integer is not considered. If the 1st integer is `3` then Tah sends the status of all analog pins to the smartphone in the following format:

{% highlight ruby %}
A0:456
A1:100
:
:
:
A5:374
{% endhighlight %}

So Tah sends the value of the analog pin together with the pin number as suffix and its value separated by a colon. Similarly, if the 1st integer is `4`, then Tah send the status of all digital pins to the smartphone in the same format as mentioned above. For example, 

{% highlight ruby %}
D2:1
D3:0
:
:
:
D13:1
{% endhighlight %}

In this way the ArdSCL protocol is used for communication between the Tah and the Smartphone.
 
 