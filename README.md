# AXI_to_AHB_Bridge
Overview:
The AXI to AHB Bridge is an open-source project that enables seamless communication between high-speed, high-bandwidth AXI masters (e.g., processors and controllers) and low-speed, low-bandwidth AHB slaves (e.g., memories and peripherals). This bridge is particularly useful in heterogeneous SoCs where AXI and AHB-compatible masters and slaves coexist.
The design is scalable, easy to integrate, and optimized for efficient protocol conversion.

Features:

    Supports high-speed AXI masters and low-speed AHB slaves.
    
    Supports single transfers and burts (fixed, incrementing and wrappinp)

    Ensure readiness of masters and slave (handles corner cases such as slave busy, master stop sending or recieving data during transaction)
    
    Enables integration in heterogeneous SoCs with AXI/AHB protocol interfaces.
    
    Arbitrate in a round robin passion between AXI write and read channels.
    
    Scalable architecture for various system requirements.
    
    Open-source contribution, open to feedback and suggestions.
    

Use Cases:

    Heterogeneous SoCs: Seamless communication between different protocol-compatible masters and slaves.
    
    Performance Optimization: Ideal for scenarios with high-performance AXI systems communicating with lower-speed peripherals.
    
    System Integration: Suitable for mixed protocol designs in embedded systems.

Design Details:

The AXI2AHB bridge translates transactions from the AXI interface into the AHB protocol. It monitors the AXI channels (AW, W, AR, R, and B) and converts them into corresponding AHB Address and Data Phases.
Key Components

    Arbitor FSM: Handles parallel read and write from AXI and start transaction by triggering AXI FSM in read or write mode in round robin passion
    
    AXI Write FSM: Handles read and write data of AXI channels
    
    AHB Write FSM: Generates AHB write and read transactions based on control signals coming from AXI FSM (Mapping of AXI transaction to an equivalent AHB Transfer)
    
    Synchronization Logic: Ensures proper handshaking between AXI and AHB protocols.
    
    Registers: Hold transaction control information for whole transaction such as (AWBURST,AWLEN,AWSIZE,AWADDR), (ARBURST,ARLEN,ARSIZE,ARADDR) and (HRESP)
    
    FIFOs: Stors incoming data from W channel of AXI incase of writing and RDATA coming from AHB incase of reading (I have used Xilinx IP for read and write FIFOs and are configured of 32 bit data, 32 bit address and 4            bit strob, however it can be reconfigured in vivado for any use case)
    
Block Diagram of the Design

![AXI_to_AHB_Bridge_Block_Diagram](https://github.com/user-attachments/assets/c0d8a7d3-0aa6-4d20-afeb-a54cee011190)

Arbitor FSM Diagram

![Arbiter_FSM_Diagram](https://github.com/user-attachments/assets/4b20c8f1-abc2-4111-ba23-2a634d56eb13)

AXI FSM Diagram

![AXI_FSM_Diagram](https://github.com/user-attachments/assets/2ba3a978-729c-46a3-b320-ebdd9ddf6e63)

AHB FSM Diagram

![AHB_FSM_Diagram](https://github.com/user-attachments/assets/9e09b06d-2f81-417f-8466-08187baf8ac0)

Code:

    The project is created in Vivado 2019.1

    The codes are written in SystemVerilog

    Project contains 3 sv files which are as:

        1) AXI_to_AHB_Bridge.sv: contains the design which has registers counters and both AXI and AHB FSMs it also has instantiation of read and write fifos and arbitor

        2) Arbitor_Read_Write.sv: round robin arbitor FSM is implemented in this file

        3) AXI_to_AHB_Bridge_tb.sv: Simple testbench with single read/write transfers (with OKAY, ERROR and wait states), burst read/write transfer and arbitor testing cases
    
    The FIFOs that is instantiated in AXI_to_AHB_Bridge.sv are Xilinx IP.
    FIFO IP is generated with depth 64 and data_width 36 (32 for data and 4 for strobes) can be configured differently as per requirement.
    If the design is intended for different vendor FPGA or ASIC then an equivalent fifo will be requitred with the signal in instantiation template.

    Code are highly commented and includes all necessary details of each logic

    
Contributions:
This project is open for contributions. I encourage:

    Suggestions for improvement.
    
    Bug reporting and fixes.
    
    Design scalability feedback.

Reach out to me at: wadoodalam44@gmail.com
