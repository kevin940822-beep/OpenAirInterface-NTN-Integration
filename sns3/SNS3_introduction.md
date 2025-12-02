# Refrence
> https://www.sns3.org/
> 
> https://github.com/sns3/sns3-satellite/blob/master/doc/satellite-design.rst

## SNS3 Introduction

- Satellite Network Simulator 3 (SNS3) is a satellite network extension to Network Simulator 3 (ns-3) platform.
- SNS3 models a full interactive multi-spot beam satellite network with a geostationary satellite (GEO).
- transparent star (bent-pipe) payload.

- reference satellite system have 72 spot-beams with an  European coverage.

- serverd by 5 gateways (GWs) and using Ka-band frequencies on the feeder/user link.

- implements the DVB-RCS2/S2 standards.
  - DVB-RCS2 (return link) : Digital Video Broadcast - Return Channel via Satellite - 2nd generation
  - DVB-S2 (forword link) : Digital Video Broadcasting - Satellite - 2nd generation

## Frame configuration (框架與時槽結構)
### Return Link (DVB-RCS2)
- Use **Multi-Frequency Time Division Multiple Access (MF-TDMA)**.
- Composed of:
  1. Superframe sequences
  2. Superframes
  3. Frames
  4. Time slots
  5. Bandwidth Time Units (BTUs)
   
- The used frame structures are dynamically configured by the **Network Control Center (NCC)**.
  - **Superframe Composition Table (SCT)**
  - **Frame Composition Table v2 (FCT2)**
  - **Broadcast Composition Table (BCT)**

- The satellite module does not explicitly model **SCT**, **FCT** and **BCT**.
- The frame configurations may be changed by **parametrization**.
- NCC models Terminal Burst Time Plan v2 (TBTPv2).
  - Is capable of configuring the time slots dynamically for each superframe (e.g. start times, durations, waveforms).

 ### Forward Link (DVB-S2)
 - Using **DVB-S2 Time Division Multiplexing (TDM)**.
 - feeder link 2 GHz bandwidth is:
   - Divided into **16 carriers**, each **125 MHz**. 
   - Each carrier is statically mapped to a user link frequency color.  ##每個 carrier 固定對應到某一個 user-beam 的 125 MHz 顏色
   - Only one carrier per beam is supported.    ##每個 beam 只支援 一個 carrier
  
## Architecture (整體架構)
<img width="1300" height="537" alt="image" src="https://github.com/user-attachments/assets/5949f5b4-03a9-46ba-9677-3c0bd824779f" />

> Refrence : https://www.sns3.org/doc/satellite-design.html#fig-satellite-general-architecture

- All satellite nodes require a new implementation of a ```SatNetDevice```
- ```SatNetDevice``` implement:
  - **Logical Link Control (LLC)**.
  - **Medium Access Control (MAC)**.
  - **Physical (PHY)**.
- A new implementation of the Channel class ```SatChannel```.
  - To support satellite system received signal power calculations.

- Satellite module implements both spherical and geodetic coordinate systems (**WGS80** and **GRS84**). 
  - In addition to labeling latitude, longitude, altitude of UT, GEO satellite and GW.

##支援球面/大地座標（WGS80、GRS84），方便標示 UT / SAT / GW 的經緯度與高度
### 1 Left side End user（地面使用者）
- Connection:
  - Use **CSMA channel** connect to User Terminal(UT).
- Protocol Stack
  - **Data Link Layer (CSMA)** : Ethernet link to the UT.
  - **Network Layer** : An Ip layer(IPv4 or IPv6), responsible for packet routing by IPv4.
  - **Transport Layer** : Use TCP/UDP end-to-end transport protocols, provides reliability, congestion control, ports.
  - **Application Layer** : Actual applications.
- This end user simply sends and receives IP traffic over a LAN interface.
- It does not know that its packets will cross a satellite system.

### 2 User Terminal (UT)
- Connection:
