---
layout: mypost
title: "Year 2 Project: Slave Computer Development"
categories: [Year 2 Project]
lang: en-us
---

Link to [Chinese Version](https://juntong20xx.github.io/posts/2025/02/10/year-2-project-%E4%B8%8B%E4%BD%8D%E6%9C%BA%E5%BC%80%E5%8F%91.html).

In embedded development, Slave computers typically don't participate in decision-making and serve as actuators or sensors.

In this project, each module functions as a slave computer executing instructions from the host computer.

**Development Setup**  

- Slave computer: Arduino Uno 
- Host computer: Raspberry Pi 5 

## Code Design

**State computer Design**

The slave computer doesn't make decisions and can be treated as a state computer:

![slave computer status computer](slave computer status machine.jpg)

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
  
  1. `ping`: Test host computer availability  
  2. `wait`: Notify host computer of standby status  
  3. `command`: Instruction execution cycle  

Protocol details available in the Year 2 Project Slave Computer Wiki.

**Debugging Design**

With no display/terminal, the onboard LED indicates status:  
- Blinking: No connection  
- Steady: Connected  

The following figure is a screenshot of the protocol design during development (translated by Google):

![Screenshot of Protocol Design Notes](Screenshot of Protocol Design Notes.png)

## Issues & Solutions
**Serial port occupation during programming**

**Analysis**: Conflict between host computer process and programming process

**Solution**: Manually close host computer process before programming  

**Intermittent connectivity** 

**Analysis**: CRC validation failures (observed via serial debugging)

**Solution**: Track last 8 ping records - consider connected if majority succeed 

![CRC ERROR](CRC ERROR.png)

## Testing
Connection test: Verify slave computer status changes when host computer starts/stops  

![测试 ping](测试 ping.jpg)

Servo control test: Validate host computer command execution  

![测试舵机](测试舵机.gif)

Serial message capture: Oscilloscope verification of communication  

![示波器照片](示波器照片.jpg)

![示波器截图](TEK0001.JPG)

Synchronized testing with host computer - see [Host Computer Development Blog](https://juntong20xx.github.io/posts/2025/02/12/Year-2-Project-Host-Computer-Development.html) for details.

![舵机在上位机操控下旋转](https://juntong20xx.github.io/posts/2025/02/12/%E8%88%B5%E6%9C%BA%E5%9C%A8%E4%B8%8A%E4%BD%8D%E6%9C%BA%E6%93%8D%E6%8E%A7%E4%B8%8B%E6%97%8B%E8%BD%AC.gif)