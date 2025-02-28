---
layout: mypost
title: "year 2 project index_en"
categories: [Year 2 Project]
lang: en-gb
---

![](Poster - Release.png)

# Year 2 Project Review: Modular Robotic Interface Platform Development

In our second-year project, our team has been dedicated to developing an innovative, flexible modular robotic interface platform. This project aims to provide a smarter and more open control solution for Internet of Things (IoT) devices.

## Project Overview

Our project primarily consists of four core components:

1. **Slave Computer System**: Based on Arduino Uno, responsible for executing specific hardware instructions.
2. **Host Computer System**: Utilizing Raspberry Pi 5, serving as the control centre for the entire system.
3. **Backend Service**: Built with FastAPI, providing device management and authentication.
4. **Frontend Interface**: Developed using Vue.js, offering an intuitive user interaction experience.

## Technical Highlights

### Flexible Communication Protocol
We designed a unique communication protocol that supports:
- Dual-channel network for rapid response
- CRC-based data validation
- Dynamic addition of personal devices

### Security Authentication
Implemented Time-based One-Time Password (TOTP) algorithm to enhance system security. Users must input both username and dynamic password.

### Open Architecture
- Fully open-source SDK
- Support for development in Python and C++
- Flexible modular interface

## Key Challenges and Solutions

During development, we overcame several technical challenges:
- Serial communication stability
- Dynamic multi-device management
- Cross-platform compatibility

Through meticulous testing and iteration, we ultimately created a stable and efficient system.

## Unique Project Features

What sets our project apart:
- Web-based remote control interface
- Modular robotic interface platform
- Low-cost implementation
- Open-source SDK allowing custom scripting
- Dual-channel network for immediate response

## Technical Stack

**Hardware**:
- Slave Computer: Arduino Uno
- Host Computer: Raspberry Pi 5

**Software**:
- Frontend: Vue.js
- Backend: FastAPI (Python)
- Communication: Custom UART protocol
- Authentication: TOTP

## Future Outlook

We believe this platform will bring innovation to the IoT and robotics fields. Users can:
- Quickly build their own robotic systems
- Flexibly control various hardware devices
- Implement personalized smart home solutions at low cost

The project code is now open-sourced on GitHub, and we welcome developers interested in this field to discuss and contribute!

## Project Achievements

- Completed full-link communication verification
- Implemented dynamic multi-device management
- Released stable version v0.1.0