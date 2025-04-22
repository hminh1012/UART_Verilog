# UART System Design and Implementation on DE2 FPGA

## Project Overview

This project involves the design and implementation of a Universal Asynchronous Receiver and Transmitter (UART) system on the Altera DE2 FPGA board. The UART system facilitates serial communication using the RS-232 standard, operating at a baud rate of 19,200 with 8 data bits, 1 stop bit, and no parity. The system is verified using a loop-back circuit connected to a PC via a serial port, with data visualized on the DE2 board's LEDs and seven-segment display.

## Repository Contents

- **Verilog Files**:
  - `fifo.v`: Implements the FIFO buffer (Listing 4.20).
  - `mod_m_counter.v`: Baud rate generator (Listing 4.3).
  - `debounce.v`: Debounce circuit for pushbutton (Listing 6.2).
  - `uart.v`: Complete UART core module (Listing 8.4).
  - `uart_test.v`: Top-level test module for verification (Listing 8.5).
- **Documentation**:
  - `Report_Midterm_UART.pdf`: Detailed project report covering design, implementation, and verification.
- **Quartus II Project Files** (optional, not included by default):
  - `.qsf`: Pin assignments and project settings.
  - `.qpf`: Quartus project file.

## System Description

The UART system consists of two main subsystems:

1. **Receiving Subsystem**:
   - Uses a 16x oversampling mechanism to detect and sample incoming bits.
   - Includes a UART receiver and a 4-word FIFO buffer.
   - States: idle, start, data, stop.
2. **Transmitting Subsystem**:
   - Serializes data using a shift register, operating at the baud rate.
   - Shares the baud rate generator with the receiver.
   - Includes a UART transmitter and a 4-word FIFO buffer.

The complete UART core integrates both subsystems with a baud rate generator and dual FIFO buffers for data flow management. The system is designed to handle ASCII characters, with a verification process that increments received data by 1 before transmitting it back.

## Verification Setup

- **Hardware**: Altera DE2 FPGA board with a Cyclone II EP2C35F896C6 FPGA.
- **Connection**: DE2 RS-232 port connected to a PC via a USB-to-RS232 adapter.
- **Software**: Hercules terminal utility for sending/receiving data.
- **Operation**:
  - Send characters (e.g., "HAL") from the PC.
  - Received data is stored in the receiver's FIFO and displayed on LEDs.
  - Pressing a debounced pushbutton (KEY\[1\]) increments the data (e.g., 'H' → 'I') and sends it back to the PC.
  - FIFO status (tx_full, rx_empty) is shown on the seven-segment display.
- **Example**: Sending "HAL" results in "IBM" being received on the PC after three button presses.

## Setup Instructions

1. **Prerequisites**:
   - Install Quartus II (version 12.0 or later).
   - Install the USB Blaster driver for DE2 board communication.
   - Install a terminal program (e.g., Hercules, PuTTY, Tera Term).
2. **Project Setup**:
   - Create a new Quartus II project:
     - Name: `UART_DE2_Test`.
     - Device: Cyclone II EP2C35F896C6.
   - Add the provided Verilog files (`fifo.v`, `mod_m_counter.v`, `debounce.v`, `uart.v`, `uart_test.v`).
   - Set `uart_test.v` as the top-level module.
3. **Pin Assignments**:
   - clk: PIN_AD15 (50 MHz clock)
   - reset: PIN_AA23 (SW\[0\])
   - btn\[0\]: PIN_T29 (KEY\[0\])
   - rx: PIN_D21 (UART_RXD)
   - tx: PIN_E21 (UART_TXD)
   - led\[7:0\]: LEDG\[7:0\]
   - sseg\[6:0\]: LEDR\[6:0\]
   - Configure in Quartus II Pin Planner or edit the `.qsf` file.
4. **Compilation**:
   - Compile the project in Quartus II (Processing → Start Compilation).
   - Fix any syntax or pin assignment errors.
5. **Hardware Setup**:
   - Connect the DE2 board to the PC via USB Blaster and RS-232 port.
   - Power the DE2 board with a 7.5V DC supply.
   - Set the RUN/PROG switch to RUN.
6. **Testing**:
   - Configure the terminal (baud rate: 19,200, 8 data bits, 1 stop bit, no parity).
   - Send characters (e.g., "HAL") and observe LEDs and FIFO status.
   - Press KEY\[1\] to process each character and verify output (e.g., "IBM") on the terminal.

## Results

- **Input**: Sending "HAL" (ASCII: 0x48, 0x41, 0x4C).
- **Output**: Receives "IBM" (ASCII: 0x49, 0x42, 0x4D) after incrementing and button presses.
- **Visualization**:
  - LEDs display received data (e.g., 0x48 for 'H' → LEDs 6 and 3 on).
  - Seven-segment display shows FIFO status (segment d for rx_empty, segment g for tx_full).

## Limitations

- The 16x oversampling limits high-speed applications.
- The 4-word FIFO buffer is suitable for low-to-moderate data rates.
- ASCII communication is limited to 7-bit codes, extended to 8 bits with MSB = 0.

## Future Improvements

- Increase FIFO buffer size for higher data throughput.
- Support variable baud rates or parity options.
- Implement error detection (e.g., framing or overrun errors).
- Enhance the interface for real-time data monitoring.

## References

- Altera DE2 User Manual.
- Quartus II Documentation.
- Verilog textbook listings (4.3, 4.20, 6.2, 8.4, 8.5).
- Hercules SETUP utility documentation.

## Contributors

- Nguyen Thi Tam
- Tran Hoang Minh
- Instructor: Nguyen Van Cuong

## License

This project is licensed under the MIT License. See the `LICENSE` file for details (if included).