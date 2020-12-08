# odgps-station
Setup and configuration for a OpenDGPS reference station

This repository bundles all software and configuration files to setup an existing environment to act as a reference station for OpenDGPS. The overall goal is to help any participant to run a number of configuration steps specific for supported hardware, software, and network environment.

## Usage

Basically the setup is splitted in two parts to calculate the differential signal. First the OS needs to be able to communicate with the GNSS receiver and second the RTKLIB have to be up and running. 

## Supported environments

A reference station for OpenDGPS is build from a computational node and a GNSS receiver (widely named as GPS-mouse).

### OpenDGPS nodes

A OpenDGPS node is a commputer running Linux and powerful enough to do the calculation for RTK (Real Time Kinetic). By this the node calculates the __D__ in OpenDGPS which is the __difference__ between the relative position provided by the satellites and the real (fix) position measured beforehand.

Connected to the node is a GNSS receiver (called widely GPS-mouse) which is cappable to give the raw data to the node via serial connection (typically USB).

#### Hardware (Boards)

|Vendor |Part identifier |Description |
--- | --- | --- |
| Raspberry | Raspberry Pi 4 | 4 ARMv8 cores with 1GByte of LDRAM |
| Texas Instruments | beagle bone | 4 ??? cores with 1GByte of RAM |
| parallela | Parallela Board | 18 Core (Epiphany, FPGA, ARMv9) 1GByte RAM  

#### Hardware (GNSS Receiver)

|Vendor |Part identifier |Description 
--- | --- | ---
Myriad | LimeSDR(USB) | Xilinx Artix 7, 100kHz to 6 GHz Rx/Tx full duplex [github: gnss-sdr/GNSS-SDR](https://github.com/gnss-sdr/gnss-sdr)
u-blox | LEA M8T | Arduino board to provide the raw data via USB

## Helpful links

- [DGPS mit RTKLIB](http://www.archeotech.de/DGPS-mit-RTKLIB/) Describes (in german) how to setup an u-blox receiver with reference data from the german public SAPOS service.
- [RTKLIB auf dem Raspberry Pi](http://www.archeotech.de/rtklib-auf-raspberrypi/)
