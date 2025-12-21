# Peak PCAN PCI QNX Resource Manager

QNX resource manager for PEAK-System PCAN-PCI CAN bus interface cards.

## Overview

This project provides a native QNX resource manager implementation for **PEAK PCAN-PCI** devices, enabling real-time CAN communication in embedded systems running **QNX Neutrino RTOS**.

The CAN frame format is defined in [can.h](include/can.h), based on the Linux SocketCAN specification (https://github.com/torvalds/linux/blob/master/include/uapi/linux/can.h):

```c
/*
 * Controller Area Network Identifier structure
 *
 * bit 0-28 : CAN identifier (11/29 bit)
 * bit 29   : error message frame flag (0 = data frame, 1 = error message)
 * bit 30   : remote transmission request flag (1 = RTR frame)
 * bit 31   : frame format flag (0 = standard 11 bit, 1 = extended 29 bit)
 */

struct can_frame {
    canid_t can_id;   /* 32-bit CAN_ID + EFF/RTR/ERR flags */
   uint8_t    len;      /* Data Length Code (0..8) */
   uint8_t    __pad;    /* padding */
   uint8_t    __res0;   /* reserved / padding */
   uint8_t    len8_dlc; /* optional DLC for 8-byte payload length (9..15) */
   uint8_t    data[CAN_MAX_DLEN] __attribute__((aligned(8)));
};
```

includes **cansend** and **candump** utilities similar to Linux can utils.

## Features

- Resource manager for PEAK PCAN-PCI cards
- Compatible with:
  - QNX 7.0
  - QNX 7.1
  - QNX 8.0
- Supports multiple architectures:
  - x86_64
  - ARM
  - ARM_64
- Modular project structure for easy integration

## Build Targets

| QNX Version | Architectures Supported |
|-------------|-------------------------|
| QNX 7.0     | x86_64, ARM, ARM_64     |
| QNX 7.1     | x86_64, ARM, ARM_64     |
| QNX 8.0     | x86_64, ARM_64          |

## Project Structure

```
├── candump/   # Utility to dump CAN messages
├── cansend/   # Utility to send CAN messages
├── include/    # Shared files
├── resmgr/    # Peak CAN resource manager (driver)
├── README.md  # Documentation
├── LICENSE    # License file
├── CHANGELOG.md
├── jamroot.jam

```

## Build

ready to build as QNX projects with project files or Boost build

### Build command line

Build using your target SDP
Set the environment variables for the appropriate QNX SDP version:
**Windows:** Run the qnxsdp_ver-env.bat script.
**Linux:** Source the qnxsdp_ver-env.sh script.

In the Project's root directory, type make. This command build all binaries for all target platforms.

## Usage

```sh
# Start resource manager
canrmd -d can1 -s 800

# Send a CAN message
cansend can1 123#DEADBEEF

# Dump CAN messages
candump can1
```

### Options

- `-d device` : Specify PCAN device node (e.g., `can0`)
- `-s bus_speed` : Bus speed in kbit per second (e.g., 125 for 125kbit/s)

Possible speeds: 10, 20, 50, 100, 125, 250, 500, 800, 1000

## Notes

- Requires PEAK PCAN-PCI hardware
- Tested on QNX 7.0, 7.1 and 8.0 with x86 platforms

## License

This project is licensed under the GPL-3.0 License. See [LICENSE](LICENSE) for details.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you’d like to change.

