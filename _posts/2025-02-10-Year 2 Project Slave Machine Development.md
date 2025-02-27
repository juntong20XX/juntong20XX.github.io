---
layout: mypost
title: "Year 2 Project: Slave Machine Development"
categories: [Year 2 Project]
lang: en-us
---

Link to [Chinese Version](https://juntong20xx.github.io/posts/2025/02/10/year-2-project-%E4%B8%8B%E4%BD%8D%E6%9C%BA%E5%BC%80%E5%8F%91.html).

In embedded development, Slave machines typically don't participate in decision-making and serve as actuators or sensors.

In this project, each module functions as a slave machine executing instructions from the host machine.

**Development Setup**  

- Slave machine: Arduino Uno 
- Host machine: Raspberry Pi 5 

## Code Design

**State Machine Design**

The slave machine doesn't make decisions and can be treated as a state machine:

![slave computer status machine](slave computer status machine.jpg)

```mermaid
stateDiagram-v2
    [*] --> Disconnected
    Disconnected --> CheckConnection: Periodic check
    CheckConnection --> Disconnected: No response
    CheckConnection --> WaitCommand: Responded
    WaitCommand --> CheckConnection: Periodic check
    WaitCommand --> FeedbackConfirm: Checksum confirmed
    FeedbackConfirm --> Execute: Verified & confirmed
    FeedbackConfirm --> CheckConnection: No response/canceled
    Execute --> WaitCommand
```

#### Communication Protocol Design

![communication system](communication system.png)

- **Physical layer** (Communication Channel): UART over USB  
  
  Advantages:  
  
  - Easy operation  
  - Reliable USB connection  
  
- **Data packet structure** (Encoder-modulator/"Link Layer"):  

![uart packet](uart packet.svg)

```mermaid
packet-beta
title UART Packet
0-7: "Start Byte"
8-15: "CRC"
16-79: "Data"
80-87: "Stop Byte"
```

- **Application layer** (Message Source): 
  
  Packet size: 16 bytes (designed for Python compatibility, equivalent to two `int` types):  
  
  ```cpp
  struct __attribute__((packed)) STRUCT_Message {
      int32_t msg_type;
      char data[4];
  } Message;
  ```
  
- **Connection protocols**:  
  
  1. `ping`: Test host machine availability  
  2. `wait`: Notify host machine of standby status  
  3. `command`: Instruction execution cycle  

Protocol details available in the Year 2 Project Slave Computer Wiki.

**Debugging Design**

With no display/terminal, the onboard LED indicates status:  
- Blinking: No connection  
- Steady: Connected  

## Issues & Solutions
**Serial port occupation during programming**

**Analysis**: Conflict between host machine process and programming process

**Solution**: Manually close host machine process before programming  

**Intermittent connectivity** 

**Analysis**: CRC validation failures (observed via serial debugging)

**Solution**: Track last 8 ping records - consider connected if majority succeed 

![CRC ERROR](CRC ERROR.png)

## Testing
Connection test: Verify slave machine status changes when host machine starts/stops  

![测试 ping](测试 ping.jpg)

Servo control test: Validate host machine command execution  

![测试舵机](测试舵机.gif)

Serial message capture: Oscilloscope verification of communication  

![示波器照片](示波器照片.jpg)

![示波器截图](TEK0001.JPG)

Synchronized testing with host machine - see [Host Machine Development Blog](https://juntong20xx.github.io/posts/2025/02/12/Year-2-Project-Host-Computer-Development.html) for details.

![舵机在上位机操控下旋转](测试舵机.gif)