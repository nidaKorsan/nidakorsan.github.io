---
layout: post
title:  "CanSat Flight Software"
date:   2019-06-01
desc: "CanSat Flight Software"
keywords: "Cansat,CanSat,Nasa,competition,flight,sattellite"
categories: [C++]
tags: [CanSat, Flight, Software, Teensy 3.6]
icon: icon-cplusplus
---

# Introduction

In my second year in CSE, I joined the Robotics and Automation club in my university and as a team, we decided to attend to CanSat Competition which is orginazed by American Astronautical Society and sponsored by U.S. Naval Research Laboratory, NASA and more organizations.

The consept of the competition design-build-launch a space-type system, following the given requirements. This space-type system is called a "*CanSat*" (short for **Can** **Sat**telite). Every year the mission changes. In the year our team had attended, we were expected to design an auto-gyro/passive helicopter recovery descent control system of a science payload and a container to protect the science payload as it is deployed from the rocket.

During this mission we were expected to provide and transmit data which include measurements for altitude using air pressure, external temperature, battery voltage, GPS position, pitch and roll and auto-gyro/ passive helicopter recovery blade spin rate to a ground control device. When the science payload lands, the software loaded should stop all telemetry and activate an audio beacon which is located on the science payload.

I was responsible for designing the flow of flight software that is needed for the quests listed above and develop this software. 

# Development

We decided to use Teensy 3.6 board to control and manage the sensors needed. Thr reasons behind choosing this board are that it is light and small, well-priced and that it has as sufficient memory and speed. Also it had more than one serial busses, which allowed the software to be a bit clearer. For telemetry we used XBEE radios.

As development environment I used Visual Basic, Arduino IDE and XCTU IDE. 

### Flight Software Design Summary
* Necessary raw data will be received from various sensors.
* Data will be sent to MCU (micro controller unit).
* Later on this data will be saved on an SD card.
* Through telemetry, the data packages will be sent to the ground station via XBEE radios.

During development of this piece of software I did lots of research to get the idea behind using sensors efficiently, communication protocols and using XBEE radios for telemetry.

## Sensor Management

Learning how to use sensors and communicating with them was a new thing for me. For some of the sensors I used some libraries that are developed by PJRC developers. For the communication between the sensors and MCU, I used I2C protocol. The advantages of using I2C protocol are:
* maintains low pin/signal count even with numerous devices on the bus
* adapts to the needs of different slave devices
* readily supports multiple masters

Figuring out how the sensors are connected, how to test them and connecting them to each other took time but in the end I could manage to work all of them harmoniously.

I used 6 sensors, an SD card and controlled multiple servo motors. The sensors are used for;
* Altitude, pressure, temperature
* Battery voltage
* Time (hh.mm.ss), latitude, longitude, altitude from GPS
* Pitch and roll

After reading the raw data from the sensors, I processed these data and filtered the necessary ones and organized them into a package to transmit later. 


## XBEE Radios

XBEE radios were needed for telemetry between the science payload and ground control device. We were expected to transmit one data package per second. Hence these devices with proper antennas were needed. 

Competitions requirements included wanted configurations for these radios such as setting their NETID/PANID and transmitting modes. For example each team were expected to transmit their data only to their team and not broadcast the packages.

To configure the XBEE radios, I used XCTU configuration platform and did research how to configure the radios to get the highest performance. The most problematic situation that occured was that in long distances, there was too much package loss. We overcome this issue by testing our system in a different fequency. 

# Result

We went to Houston, Texas and after mandatory measurements done by executives, we launched our project to 1000 meters high and observed the results. Due to an unexpected error, the system got stuck at one point but the telemetry was fine there was a small to none package loss. We ranked 25th in 40 teams over the world.

## Personal Experience

I learned a lot from this project including team work, being patient *and* consistent during developement of software and new fields including sensor and MCU management, testing and developing code for them. It was a wonderful journey alongside a hardworking team and an experienced supervisor Dr. Fuad ALIEW.

The link to [GTU-SAT](http://gtusat.gturobotik.com)