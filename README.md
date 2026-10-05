# VMO - Home Monitoring Robot

CIE 349 Embedded Systems project combining PIC16F877A firmware, ESP32 control and telemetry, hardware design files, and a ROS 2 environment blueprint for Raspberry Pi vision integration.

The project targets home pet monitoring and environmental sensing. This repository contains the submitted project package; it is an academic prototype rather than a validated home safety system.

## Included Work

- PIC16F877A motor control, PWM, UART, sensor drivers, and cooperative task scheduling.
- ESP32 FreeRTOS tasks, browser-based robot controls, sensor telemetry, and serial messaging.
- Temperature/humidity sensing, MQ-2 gas sensing, and ultrasonic distance sensing.
- Generated Nanopb message definitions for robot state.
- KiCad PCB design archive and a Proteus simulation project.
- Technical report and a vision model summary.

## Repository Layout

| Path | Contents |
| --- | --- |
| [`1_Report/`](1_Report/) | Technical project report |
| [`2_Software_and_Firmware/PIC_Code/`](2_Software_and_Firmware/PIC_Code/) | PIC C firmware, driver layers, and build script |
| [`2_Software_and_Firmware/ESP32_Code/ESP_VMO/`](2_Software_and_Firmware/ESP32_Code/ESP_VMO/) | Arduino sketch, generated message files, and configuration example |
| [`2_Software_and_Firmware/Dockerfile`](2_Software_and_Firmware/Dockerfile) | ROS 2 Humble container environment |
| [`3_Hardware_and_Simulation/`](3_Hardware_and_Simulation/) | PCB archive and Proteus project |
| [`4_Model/`](4_Model/) | Vision model summary and references |

## Firmware Variants

The PIC code provides a layered implementation using `APP`, `MCAL`, `HAL`, and `SERVICES`. The ESP32 sketch also contains direct motor and sensor control. Review pin assignments and serial protocol behavior before connecting the two implementations; the files should not be assumed to represent one verified combined firmware release.

The PIC configuration uses a 20 MHz oscillator and a DHT11 interface. The ESP32 sketch selects DHT22 and uses board-specific pins. Match the selected firmware to the actual hardware and simulation.

## PIC Build

Install the Microchip XC8 compiler and make `xc8-cc` available on your command path. The supplied script expects a device pack at `PIC16F_DFP/xc8` relative to `PIC_Code`; install that pack or adjust the script's device-pack path.

```powershell
cd 2_Software_and_Firmware/PIC_Code
.\build.bat
```

The expected output is `output/Driver.hex`. Build outputs and locally installed device packs are excluded from version control.

## ESP32 Setup

1. Install the Arduino ESP32 board package and select the board matching the wiring.
2. Install the libraries referenced by the sketch: PacketSerial, the DHT sensor library, and Nanopb. The included `libraries_used.txt` is an original submission note; the sketch's includes determine the actual dependencies.
3. Copy `wifi_config.example.h` to `wifi_config.h` beside `ESP_VMO.ino`, then enter your local network settings.
4. Open `ESP_VMO.ino`, verify the motor and sensor pins, and compile/upload using the matching ESP32 toolchain. The PWM code targets the ESP32 Arduino core 3.x API.

`wifi_config.h` is ignored by Git. The browser dashboard is served by the ESP32 on port 80 after network startup.

## ROS 2 Environment

With Docker installed:

```powershell
docker build -t vmo-ros2 2_Software_and_Firmware
docker run --rm -it vmo-ros2
```

The Dockerfile prepares a ROS 2 Humble environment, installs vision and communication dependencies, and clones an external YOLO C++ project. It does not include a complete Raspberry Pi application or ROS node implementation.

## Reports And Hardware

- [Technical report](1_Report/VMO_Technical_Report.pdf)
- [Vision model summary](4_Model/Model_Summary_and_Links.pdf)
- [KiCad PCB archive](3_Hardware_and_Simulation/VMO_KiCad_PCB.zip)
- [Proteus simulation](3_Hardware_and_Simulation/VMO_Proteus_Simulation.pdsprj)

Open hardware files with their corresponding design tools. The submitted package contains no ONNX model file, despite its mention in the original `README.txt`.

## Publication Status

Published as a private repository to preserve the original package's source-privacy intent. The publishing copy removes embedded Wi-Fi credentials and supplies an ignored local configuration file instead. Firmware builds, simulation, and physical hardware operation have not been verified during publication.
