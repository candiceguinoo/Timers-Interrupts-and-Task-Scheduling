# Laboratory Activity 1: Timers, Interrupts, and Task Scheduling

**Course:** BCA143 Firmware Programming  
**Student:** Candice S. Guinoo
**Date:** 

---

## Project Description

This project implements timer-based interrupts and a cyclic executive scheduler on the STM32F407ZGT6 microcontroller using the RT-Thread RT-Spark development board.

The system demonstrates the use of periodic timer interrupts, external interrupts, and a foreground/background scheduling architecture to execute tasks and analyze timing behavior in embedded systems.

---

## Hardware

- RT-Thread RT-Spark Development Board (STM32F407ZGT6)
- LED PF11 (Red)
- LED PF12 (Blue)
- User Button PA0
- Debug Pin PE0

---

## Features

- TIM2 configured as a **1 Hz periodic interrupt** (High Priority)
- TIM3 configured as a **2 Hz periodic interrupt** (Medium Priority)
- External interrupt triggered by user button press
- Foreground/Background system architecture
- Timing measurement and response analysis
- Cyclic executive task scheduling

---

## System Architecture

### Interrupt Service Routines (ISR)

| Interrupt | Function | Priority |
|------------|------------|------------|
| TIM2 | Triggers Task A every 1 second | High |
| TIM3 | Triggers Task B every 0.5 seconds | Medium |
| EXTI0 | Handles button press events | Low |

### Tasks

| Task | Description | Execution Time |
|--------|-------------|---------------|
| Task A | Triggered by TIM2 interrupt | 50 ms |
| Task B | Triggered by TIM3 interrupt | 20 ms |
| Button Task | Handles external interrupt events | Event-driven |

---

## Timing Measurements

| Parameter | Definition | Measured Value |
|------------|------------|---------------|
| TRelease(TIM2) | Timer interrupt occurrence | 1.0 s |
| TLatency(Task A) | Delay before Task A starts | 24.94 µs |
| TISR(TIM2) | ISR execution time | 4.31 µs |
| TTask(Task A) | Task A execution time | 50 ms |
| TResponse(Task A) | Total response time | 50.02443 ms |

---

## Timing Calculations

### Response Time of Task A

```
TResponse(Task A)
= TLatency + TTask
= 24.43 µs + 50 ms
≈ 50.02443 ms
```

---

## Sequence Diagram

> Insert the sequence diagram image from your laboratory report here.

```text
TIM2 Interrupt
      │
      ▼
   ISR Executes
      │
      ▼
 Flag Set
      │
      ▼
 Main Loop Detects Flag
      │
      ▼
 Execute Task A
      │
      ▼
 Clear Flag
```

---

## Build Instructions

### 1. Open Project

Open the project in **STM32CubeIDE**.

### 2. Build

Navigate to:

```
Project → Build All
```

### 3. Connect Hardware

Connect the RT-Spark board via USB.

### 4. Upload Program

Navigate to:

```
Run → Debug
```

or

```
Run → Run
```

---

## Analysis

### 1. What is the maximum latency for Task A in your system?

The worst-case latency is **70 ms**. This occurs when the interrupt fires while both Task A (50 ms) and Task B (20 ms) are already executing, forcing Task A to wait until the scheduler returns to check the task flag.

### 2. If Task B is running when TIM2 interrupt occurs, how does it affect TLatency(Task A)?

The ISR immediately sets the task flag, but the main loop cannot execute Task A until Task B completes. Therefore, Task A's latency increases by the remaining execution time of Task B, up to **20 ms**.

### 3. Calculate the worst-case response time for Task A if all other tasks are running.

```
Worst-case Response Time
= Maximum Latency + Task Execution Time
= 70 ms + 50 ms
= 120 ms
```

Therefore, the worst-case response time is **120 ms**.

### 4. How would response time change with a preemptive scheduler?

With a preemptive scheduler, the currently running task would be interrupted immediately when the higher-priority task becomes ready. This would significantly reduce latency and make response times more predictable.

---

## References

1. Castor, P. R. P. (2025). *Software Design Basics [Lecture 2]*. BCA143 Firmware Programming, MSU-IIT.
2. STMicroelectronics. (2024). *STM32F4 HAL Driver User Manual*.

---

## Repository Structure

```text
Timers-Interrupts-and-Task-Scheduling/
│
├── Core/
├── Drivers/
├── STM32CubeIDE/
├── README.md
└── Documentation/
```

---

## Author

**Candice S. Guinoo**  
BCA143 Firmware Programming
