# Refrence
> https://www.sns3.org/
> 
> https://github.com/sns3/sns3-satellite/blob/master/doc/satellite-design.rst

## SNS3 Introduction

- **Satellite Network Simulator 3 (SNS3)** is a satellite network extension to **Network Simulator 3 (ns-3)** platform.
- **SNS3** models a full interactive multi-spot beam satellite network with a **geostationary satellite (GEO)**.
- transparent star (bent-pipe) payload.

- reference satellite system have **72 spot-beams with an  European coverage**.

- serverd by 5 **gateways (GWs)** and using **Ka-band frequencies** on the feeder/user link.

- implements the **DVB-RCS2/S2** standards.
  - **DVB-RCS2 (return link)** : Digital Video Broadcast - Return Channel via Satellite - 2nd generation
  - **DVB-S2 (forword link)** : Digital Video Broadcasting - Satellite - 2nd generation

## Frame configuration (框架與時槽結構)
### Return Link (DVB-RCS2)
- Use **Multi-Frequency Time Division Multiple Access (MF-TDMA)**.
- Composed of:
  1. **Superframe sequences (SFS)**
  2. **Superframes**
  3. **Frames**
  4. **Time slots**
  5. **Bandwidth Time Units (BTUs)**

**Superframe sequence (SFS)** : An ordered time sequence of superframes on a given set of carriers, identifying one **MF-TDMA** resource block.  *(在一組特定頻寬上的連續 superframe 時間序列，代表一整塊 MF-TDMA 資源)*

<img width="600" height="537" alt="image" src="https://github.com/user-attachments/assets/ccd39297-2004-4637-9320-de07b7d87531" />

   
- The used frame structures are dynamically configured by the **Network Control Center (NCC)**.
  - **Superframe Composition Table (SCT)**
  - **Frame Composition Table v2 (FCT2)**
  - **Broadcast Composition Table (BCT)**

- The satellite module does not explicitly model **SCT**, **FCT** and **BCT**.
- The frame configurations may be changed by **parametrization**.
- **NCC** models **Terminal Burst Time Plan v2 (TBTPv2)**.
  - Is capable of configuring the time slots dynamically for each **superframe** **(e.g. start times, durations, waveforms)**.

 ### Forward Link (DVB-S2)
 - Using **DVB-S2 Time Division Multiplexing (TDM)**.
 - feeder link 2 GHz bandwidth is:
   - Divided into **16 carriers**, each **125 MHz**. 
   - Each carrier is statically mapped to a user link frequency color.  *(每個 carrier 固定對應到某一個 user-beam 的 125 MHz 顏色)*
   - Only one carrier per beam is supported.    *(每個 beam 只支援 一個 carrier)*
  
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

*(支援球面/大地座標（WGS80、GRS84），方便標示 UT / SAT / GW 的經緯度與高度)*
### 1. Left side End user（地面使用者）
- Connection :
  - Use **CSMA channel** connect to User Terminal(UT).
- Protocol Stack :
  - **Data Link Layer (CSMA)** : Ethernet link to the UT.
  - **Network Layer** : An Ip layer(IPv4 or IPv6), responsible for packet routing by IPv4.
  - **Transport Layer** : Use TCP/UDP end-to-end transport protocols, provides reliability, congestion control, ports.
  - **Application Layer** : Actual applications.
- This end user simply sends and receives IP traffic over a LAN interface. *(這個 End user 只是透過 LAN 介面收送 IP 流量)*
- It does not know that its packets will cross a satellite system.

### 2. User Terminal (UT)
- Connection :
  - Use **SatChannel** connect to Satellite node.
- Protocol Stack :
  - ```SatNetDevice``` : Special satellite network interface towards the satellite.
  - **Network layer** :
    - An IP routing/forwarding layer that connects the **LAN side (CSMA)** to the **satellite side** (```SatNetDevice```).
    - Looks like a router: one interface to the **local LAN**, one interface to the **satellite network**.
  - ```GeoPosition``` : Stores the **UT’s geographic coordinates**(latitude, longitude, altitude), used to compute propagation delay, path loss, and for possible handover.    *(計算傳播延遲、路徑損耗，以及將來可能做 handover。)*

- The UT plays the role of a customer premises terminal. *(UT 扮演的是一個 用戶端終端設備／小型路由器 的角色)*
- It routes IP packets between the **local LAN** and the **satellite link**.

### 3. Satellite node
- Connection :
  - Use **SatChannel** to build **uplink/downlink** with  **UT** and **GW**.
- Protocol Stack :
  - ```SatGeoNetDevice``` :
    - Device representing the **satellite’s radio payload**.    *(代表衛星上的射頻酬載。)*
    - Does not run IP or transport protocols.
    - Receives bursts from UTs/GWs on the ```SatChannel```.
    - Forwards the bursts towards the opposite direction (UT↔GW).    *(將 burst 轉發到另一方向（UT ↔ GW）)*
    - Applies propagation effects and computes received signal quality (e.g., SINR).    *(套用傳播效應，計算收到訊號的品質（例如 SINR）。)*
  - ```GeoPosition``` : The satellite’s orbital position (e.g., GEO longitude/latitude/altitude) used to compute delays and link budgets.
 
 - The satellite is essentially a transparent relay at the **physical**/**MAC** level. *(做透明轉發的中繼站)*



<!--Internally includes : *(內部包含)*
    - **Logical Link Control (LLC)** : Packs/Unpacks higher-layer packets into **bursts**. *(把上層封包打包成 **burst**、或從 **burst** 還原出封包。)*
    - **Medium Access Control (MAC)** : Obeys the satellite access rules (frames, time slots, TBTP). *(遵守衛星系統的存取規則。)*
    - **Physical (PHY)** : Transmits bursts over the ```SatChannel``` using given waveforms/MODCODs. *(用指定的 waveform / MODCOD在 ```SatChannel``` 上發射 **burst**。)* 
-->

