---
layout: mypost
title: "Year 2 Project Review: Modular Robotic Interface Platform"
categories: [Year 2 Project]
lang: en-gb
---

![](Poster - Release.png)

In our second-year project, our team has been dedicated to developing an innovative, flexible modular robotic interface platform. This project aims to provide a smarter and more open control solution for Internet of Things (IoT) devices.

## Project Overview

Our project primarily consists of four core components:

1. **Slave Computer System**: Based on Arduino Uno, responsible for executing specific hardware instructions.
2. **Host Computer System**: Utilizing Raspberry Pi 5, serving as the control centre for the entire system.
3. **Backend Service**: Built with FastAPI, providing device management and authentication.
4. **Frontend Interface**: Developed using Vue.js, offering an intuitive user interaction experience.

Video Link: [project video](https://theuniversityofliverpool-my.sharepoint.com/:v:/g/personal/sgjzhu25_liverpool_ac_uk/EVTm5ax5ei1Im1mfhsLeLYgB1dxshit3WMcHoUkk1NLD7Q?e=q9Nbhb) (login request).

## Technical Highlights

### Flexible Communication Protocol
We designed a unique communication protocol that supports:
- Dual-channel network for rapid response
- CRC-based data validation
- Dynamic addition of personal devices

![](https://juntong20xx.github.io/posts/2025/02/10/%E7%A4%BA%E6%B3%A2%E5%99%A8%E7%85%A7%E7%89%87.jpg)

### Security Authentication
Implemented Time-based One-Time Password (TOTP) algorithm to enhance system security. Users must input both username and dynamic password.

![](https://juntong20xx.github.io/posts/2025/02/20/Login%20Page.png)

### Open Architecture
- Fully open-source SDK
- Support for development in Python and C++
- Flexible modular interface

## Project Achievements

- Completed full-link communication verification
- Implemented dynamic multi-device management
- Released stable version

## Members:

Juntong Zhu (Team Leader): Design, Software, Hardware

Haozhe Deng: Hardware

Al-Mahmoud Abdulrahman

## Other Links:

**Technology Blogs**

- [Slave Computer Development](https://juntong20xx.github.io/posts/2025/02/10/Year-2-Project-Slave-Computer-Development.html)
- [Host Computer Development](https://juntong20xx.github.io/posts/2025/02/12/Year-2-Project-Host-Computer-Development.html)
- [Backend Development](https://juntong20xx.github.io/posts/2025/02/20/Year-2-Project-Backend-Development.html)
- [Frontend Development](https://juntong20xx.github.io/posts/2025/02/20/Year-2-Project-Frontend-Development.html)
- [Host-Slave System Integration Testing](https://juntong20xx.github.io/posts/2025/02/22/Year-2-Project-Final-Version-Host-Slave-System-Integration-Testing.html)
- 

**Ramblings**