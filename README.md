# odgps-station
Setup and configuration for a OpenDGPS reference station

This repository bundles all software and configuration files to setup an existing environment to act as a reference station for OpenDGPS. The overall goal is to help any participant to run a number of configuration steps specific for supported hardware, software, and network environment.

As described in [OpenDGPS/opendgps-doc](/opendgps/opendgps-doc) the role of a reference station is to provide the differential data to users nearby to enable them to remmove GNSS signal distortions from actual data to calculate its own position with a much higher accuracy. The accuracy based on differential data from reference stations depends on the distance and number of the stations from the receiving mobile device. Typically differntial data from reference stations more than 30km away may have no effect. Weather and area also have impact.

If a new (or moved) reference station is registered on the OpenDGPS network it should be calibrated. The first calibration phase will be done automatically in the first three hours without any interaction with the network. If the administrator of the reference station confirms the coordinates of this calculation, the reference station tries to register on the OpenDGPS network with the configured API-key. If the registration is successful the station will try to get differential data to precise it's own position. Until this process is finished the station will marked as callibration level 'SELF' to permit it from sending the data for callibration purposes to other devices. 

After 24h and a heuristics analysis if the data match stability tresholds the station gets the callibration level 'BASIC' and is accepted to provide data to other user devices. 

A station can become a 'PROOVEN' calibration level by a process where the poosition of the antenna and the quality (mainly stability) of the installation is manually examined by other member of the OpenDGPS network in person. Differential data from PROOVEN reference stations have an highe impact by the calculation of the differential data provided to the user.

### Notes about privacy

The OpenDGPS network is not sending the exact locations of any reference stations to end-users device. If a request comes in from a device – either a mobile phone or another reference station – the server of the OpenDGPS network picks one or more – depends on the RTCM protocol – pseudolocations and calculates the differential data based on the real existing reference stations registered for this area. Due to the randomness in theory the location of the pseudo reference station can be exact the same as a real station but the next request will always provide another position.

If a station is registered as 'PRIVATE' the location data of this station will be encrypted in the database and additionally a fuzzy location will be stored. To fuzzy the original data the latitude and longitude will be rounded to five positions after decimal point. In effect the reference station can be anywhere in an area of around 6km east-west and 10km north-south. The station can still be used as a reference station by checking if this area is relevant for given request and if yes to decrypt the data on the fly using the differential data in the processors register and clear the register immediately.    

## Usage

Basically the setup is splitted in three parts to calculate the differential signal. First the OS needs to be able to communicate with the GNSS receiver, second the RTKLIB have to be up and running and third the station needs connectivity to the OpenDGPS network. 

## Supported environments

A reference station (called node) for OpenDGPS is build from a computational system and a GNSS receiver (widely named as GPS-mouse).

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
