---
layout: mypost
title: "Year 2 Project: Host Computer Development"
categories: [Year 2 Project]
lang: en-us
---

Link to [Chinese Version](https://juntong20xx.github.io/posts/2025/02/12/year-2-project-%E4%B8%8A%E4%BD%8D%E6%9C%BA%E5%BC%80%E5%8F%91.html).

![上位机项目 IDLE 截图](上位机项目 IDLE 截图.png)

In embedded system development, the host computer refers to the device that interacts with the slave computer, typically acting as the controller.

In this project, the host computer uses a Raspberry Pi 5, and the salve computer is an Arduino Uno, connected via USB.

![connected via USB](上位机与下位机通过 USB 连接.jpg)

## Code Design

The workflow of the host computer can be simplified as: responding to salve computer requests.

- Reply with connection confirmation when receiving ping command
- Send instructions from the command queue when receiving WaitCommand 
- Requeue instructions when receiving execution failure reports

![Host Computer Code SD](Host Computer Code SD.svg)

```mermaid
stateDiagram
    [*] --> WaitForRequest

    WaitForRequest --> HandlePing : Receive ping command
    HandlePing --> WaitForRequest : Send connection response

    WaitForRequest --> HandleWaitCommand : Receive WaitCommand
    HandleWaitCommand --> WaitForRequest : Send next instruction

    WaitForRequest --> HandleFailure : Receive failure report
    HandleFailure --> WaitForRequest : Requeue instruction

    state HandlePing {
        Send_connection_response
    }

    state HandleWaitCommand {
        Check_command_queue
        Send_next_instruction
    }

    state HandleFailure {
        Log_error
        Rollback_command_to_queue
    }
```

Additionally, to package the host computer as an SDK, we configured pyproject.toml for easy distribution as a Python module.

## Issues & Solutions

### Parameter Parsing Exception
**Analysis**: salve computer packets only use first 8 bytes as valid data, causing parsing issues in Python's strong typing system which requires handling all 16 bytes.  

**Solution**: Modify salve computer to write zeros in unused bytes.

### Serial Port Not Found  
**Analysis**: Development environment used Linux (Raspberry Pi OS) via VS Code Remote, while debugging used Windows (laptop).  

**Solution**: Add serial port parameter configuration in test code.

## Testing

Created test programs in `/test` directory that:
1. Prompt for serial port address
2. Create command queue using Python built-in list
3. Pass queue and port address to communication function
4. Run as separate thread
5. Enter main loop accepting servo rotation degree inputs

host and salve computer testing is synchronized. Refer to the salve computer development blog for more details.

Below is a screenshot from testing video:

![舵机在上位机操控下旋转](舵机在上位机操控下旋转.gif)