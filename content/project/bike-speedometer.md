---
title: "Creating a Bike Speedometer with GreenPAK and a Magnetic Sensor"
description: "This project focuses on creating an accurate bike speedometer using a GreenPAK and a magnetic sensor. Two methods are used to approach this application: the first uses the frequency detect blocks of the SLG46536V, the second method centers on a RPM counter design. The nominal resolution of this application is 1 m/s but can be changed depending upon the number of magnets used on the wheel."
tags: ["ic design", "sensor", "speedometer"]
weight: 3
draft: false
image: /img/speedometer-fig8.png
---

this writting, have published on Dialog Semiconductor website:

https://www.dialog-semiconductor.com/AN-CM-249

## Introduction

The main goal of this application is the measurement result of rotating object, such as speed of bicycle wheel. The system should have ability to interpret measurement result to be international unit of speed, namely meters per second. To realize this system, we will attach a sensor device on the bicycle. And then, our design by using some logical operation, will update the output periodically or simultaneously to interpret measurement result using 4 until 6 binary number and have precision up to 1 m/s. 

The input of the system is digital signal from magnetic switch. Every single cycle of wheel represents 2 meters traveled (circumference of wheel). For this purpose, we have implemented two alternative design. The first design is using CNT block for every grade of speed. And another design is only using digital logic with any other supported block, such as DFF block, and binary number increment. As the output, the system will update simultaneously for every change if we use the first method. Or if use second method, measurement result will be updated periodically.

The key advantages using this Dialog product is smaller, cheaper, simpler, and easy to develop. If we want to develop it more, for example making electric bicycles with controlled speed, this solution is the best one. Other products may already be widely spread in the market. But commonly it hard to develop. Besides that, usual microcontroller also can be used to implement this system, but power consumption and dimension make Dialog Product is better than them all.

Using Dialog IC will make this design be really simple, light weight and just need small space to implement it on every kind of application. For my application, for bicycle, this design can be better alternative solution even speedometer for bike cycle is ready to buy on the gallery shop of bikers. Cheaper and can less power consumption may be this advantage from others speedometer from gallery shop. And surely, this design can be integrated to more complex design system.

The system can be proven by inspect digital output signal, for this application we use 4 to 6 of LEDs. Each LED represent single bit of binary sequence. We will test it by using any radial speed of wheel. And to show this solution better than others solution, we can evaluate dimension, availability to be developed, and other parameter of comparison.

## Designed Solution

There are any alternative designs can be chosen to create simple bike speedometer. For this application note we will try two designs. Each design has advantages and deficiency. First design, we use a feature that’s only there on GreenPak 5 product, namely frequency detector. And another design, we use increment of binary number for period of time. 

### 1.First Design: Using Frequency Detector

These is one of our reasons choose SLG45636. It’s because from provided IC by Silego, this IC has most amount of counter/delay/frequency detector block. Frequency detector act like a frequency filter. It can be configured by set counter data. From the counter data, periods will be generated. For this IC, there two kind of frequency detector, namely 8-bit and 16-bit. Surely to generate more precision measurement, more bit will be better than less. For this application we will use both.  

The main rule of frequency detector is: If the input of this block has frequency more than the configured frequency, it will out high digital.  For example, we configured 16-bit CNT to frequency detector as below. 


<img class="center" src="/img/speedometer-fig1.png">

<p style="text-align: center;">Figure 1. Frequency Detector Configuration for speed 1 m/s (left side) and 2m/s (right side)</p>
 
Wheel circumference is 2,060 m. When wheel of bike rotates at least one times in 2,060 s, it means that horizontal speed of bike is more than 1 m/s. Because of that, we set counter data to 803 with clock source is OSC0 CLK/64. By provided formula, period of block will be 2,05824 s. It’s the closest value to 2,060 s.

Similar method we do to configure second frequency detector to detect speed of bike more than 2 m/s. The wheel of bike should rotate at least one times in 1,030 s. Because of that we set counter data to 401 with clock sources is OSC0 CLK/64, to make period of block will be 1,02912 s. It’s the closest value to 1,030 s. 

Above methods will be implement to another speed, such as 3 until 7 m/s for one IC for this application. From above methods, output of each frequency detector will be described as table below. 

<p style="text-align: center;">Table 1. Frequency Detector Output</p>

<img class="center" src="/img/speedometer-tab1.png">

Surely if we want to measure more than 7 m/s, we can extend it by add one more IC. So, the maximum speed will be up to 14 m/s. This speed is fast enough for a bike. 
Below is full design for First Design: Using Frequency Detector.

#### A.Simple Bike Speedometer with Binary Output and 0 until 7 m/s of speed range


<img class="center" src="/img/speedometer-fig2.png">

<p style="text-align: center;">Figure 2. Greenpak Design for first method with measurement range 0 to 7 m/s</p>

For this application, we use SLG46536V. On this IC, we configure CNT/DLY as frequency detector, which is the main component block for this application. For this application, we only use one input pin (PIN2) to get impulse signal from magnetic switch when wheel is rotating. For design above, we can measure 0 until 7 m/s of speed. Every increasing of speed by 1 m/s uses one counter block to detect it. So, for this design we only can measure until 7 m/s with 1m/s precision. Surely, it’s able scale up or extend it by adding more IC. The digital circuit on this design converts bit from Table 1 to be 3-bit binary sequence. 

<p style="text-align: center;">Table 2. Input (frequency detector output) and output of digital circuit</p>

<img class="center" src="/img/speedometer-tab2.png">

<p style="text-align: center;">Table 3. Configuration table that have been implemented to IC using GreenPak application with 1 to 7 m/s</p>

<img class="center" src="/img/speedometer-tab3.png">

#### B.Extended Version of Simple Bike Speedometer with Binary Output and speed range from 0 to 14 m/s

From design before, we know that maximum speed is only up to 7 m/s. So, by adding one more IC we will scale up the maximum speed up to 14 m/s. Below is GreenPak design and configuration that have been implemented to SLG46536V.

<img class="center" src="/img/speedometer-fig3.png">

<p style="text-align: center;">Figure 3. Greenpak Design for extended of first method with measurement range 8 to 14 m/s</p>

<img class="center" src="/img/speedometer-tab4.png">

<p style="text-align: center;">Table 4. Configuration table that have been implemented to IC using GreenPak application with 8 to 14 m/s.</p>

Mostly, design for extended IC is similar to main IC. But output of the main IC will also be the input of extended IC. The overall system using two IC for this application shown by diagram below. 

<img class="center" src="/img/speedometer-fig4.png">

<p style="text-align: center;">Figure 4. System diagram for using frequency detector with double IC</p>


### 2.Second Design: Increment of Binary Number

The second design is totally different to the first. On this design we are using full adder digital circuit. Every cycle of wheel will increase the counter number by one. The counter will reset for periods of time. For this design we use 2060ms as time period, that also will be refresh-rate of measurement result. So, if there are only one cycle in 2060 ms, it means bike’s speed is only 1 m/s. If there are 2 cycles in 2060 ms, it means bike’s speed is 2 m/s, and so on. 

<img class="center" src="/img/speedometer-fig5.png">

<p style="text-align: center;">Figure 5. GreenPak Design using increment of binary number</p>

In this design, the maximum speed can be measured up to 63 m/s (6-bit output). There are many option to optimize this method based on our need. For Example, we need to improve the measurement accuracy up to 1 km/h. It’s also possible, but the output will update for every 2060x3.6 = 7416 ms. Of course, it’s not a good idea. Another choice is improving the accuracy up to 2 km/h and update the output every 3708 ms. So, the measurement result will be multiplication of 2 km/h such as, 2, 4, 6, 8, 10 and so on. And the last choice that also may be implemented is 3 km/h of accuracy and update the output every 2472 ms. System diagram of this method, can be drawn as below. 

<img class="center" src="/img/speedometer-fig6.png">

<p style="text-align: center;">Figure 6. System diagram for increment method</p>

## Result

Now we have two alternative design for our need. So, let us compare both of that.

No	Objective	First Design	Second Design
1	Measurement result update period	Real time	Every 2060 ms 
2	Measurement Range for this application	0 to 7 m/s for each IC	0 to 63 m/s for each IC

After do some comparison between the first design and second design, second design is relatively better for this application. So, for the next capture, we will focus on the second design, namely using increment method. 


<img class="center" src="/img/speedometer-fig7.png">

<p style="text-align: center;">Figure 7. Simulate circuit design using Silego development board</p>

<img class="center" src="/img/speedometer-fig8.png">

<p style="text-align: center;">Figure 8. Prototyping circuit by using LED for 5-bit output</p>



<img class="center" src="/img/speedometer-fig91.png">

<p style="text-align: center;">Figure 9. Magnet and magnetic switch attached to bicycle</p>

Usually, speedometer sensor attached on front of wheel. But, for this testing, we are using back wheel to simplify our measurement monitor. I think it’s not a problem. So, below are the measurement results for each speed of wheel. There are 5 LEDs. The Most Significant Bit is interpreted by LED on the left side.

<img class="center" src="/img/speedometer.gif">
 
We can see on figures above that measurement result are 1 m/s, 2 m/s until 13 m/s. 


## that's All, Thank you!