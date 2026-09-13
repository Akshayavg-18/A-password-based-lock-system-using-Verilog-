# Password-Based Lock System with Stepper Motor

## Overview

This project implements a password-based access control system using Verilog HDL. The system verifies a two-digit password and, when the correct password is entered, activates an LED and drives a stepper motor through a predefined stepping sequence.

The design was implemented and tested on an FPGA platform, with timing and device-utilization analysis performed to evaluate the hardware implementation.

## Objectives

- Design a password verification system using Verilog HDL.
- Control a stepper motor based on password authentication.
- Provide visual access indication using an LED.
- Implement a clock-based delay for controlled motor speed.
- Verify the design through simulation and FPGA implementation.

## Working Principle

The system accepts two 2-bit inputs representing the password digits.

The predefined correct password is:

- Digit 1 = 1
- Digit 2 = 2

### Incorrect Password

- LED remains OFF.
- Stepper motor remains OFF.
- Stepper output is `0000`.
- Internal counters are reset.

### Correct Password

- LED turns ON.
- Stepper motor starts rotating.
- Motor follows the sequence:

text
1000 → 1100 → 0110 → 0011 → repeat

       +---------------------+
       |   Password Inputs   |
       |  digit1    digit2   |
       +----------+----------+
                  |
                  v
       +---------------------+
       | Password Comparator |
       +----------+----------+
                  |
          +-------+-------+
          |               |
        Wrong           Correct
          |               |
          v               v
   +-------------+   +-------------+
   |  Motor OFF  |   |   LED ON    |
   |   LED OFF   |   |  Motor ON   |
   +-------------+   +------+------+
                           |
                           v
                  +-----------------+
                  |  Delay Counter  |
                  +--------+--------+
                           |
                           v
                  +-----------------+
                  | Stepper Control |
                  | 1000 → 1100     |
                  | 0110 → 0011     |
                  +-----------------+

Verilog Implementation
Main Module
The main module of the project is:
password_motor_control

Inputs
Signal	Width	Description
clk	1-bit	System clock
reset_n	1-bit	Active-low reset
digit1	2-bit	First password digit
digit2	2-bit	Second password digit


Outputs
Signal	Width	Description
stepper	4-bit	Stepper motor control signals
correct_password_led	1-bit	Indicates correct password


Stepper Motor Control
The design uses a four-step full-step sequence:
Step 0 → 1000
Step 1 → 1100
Step 2 → 0110
Step 3 → 0011
After the fourth step, the sequence returns to the first step.
The motor speed is controlled using a parameterized delay counter:
parameter DELAY = 24'd10_000_000;
Results
Wrong Password
LED     → OFF
Motor   → OFF
Stepper → 0000
Correct Password
Password → 1, 2
LED      → ON
Motor    → Rotates
Sequence → 1000 → 1100 → 0110 → 0011 → repeat
FPGA Implementation Results
The design was implemented on the following FPGA device:
Device: Xilinx Spartan-3
Resource Utilization
Resource	Used	Available	Utilization
Slices	37	3584	1%
Slice Flip-Flops	29	7168	<1%
4-input LUTs	69	7168	<1%
I/O	11	141	7%
GCLK	1	8	12%
