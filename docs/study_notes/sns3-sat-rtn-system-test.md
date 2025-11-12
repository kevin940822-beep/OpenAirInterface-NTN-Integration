# SNS3 sat-rtn-system-test-example.cc
>### Refrence 
>https://github.com/sns3/sns3-satellite/blob/0fc2b8c74f0d9c2b0c3ee4ed132064a40ad2daf1/examples/sat-rtn-system-test-example.cc
>
### Result :

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/c60f707a-41df-4b89-bdd8-7bb5311ae607" />

# Table of Contents
1. [UT/SAT/GW](ut/sat/gw)
2. RTN 上行/回傳鏈路
3. Regenerator/RTN System
4. MAC 層排程與接入控制
5. 物理層（PHY）建模
6. 時槽與超幀結構
7. 網路層與應用層的回傳效能
8. 在 SNS3 模擬中的應用

---
## 1.UT/SAT/GW
- UT(User Terminal) : 使用者終端

- SAT(Satellite Network) : 衛星

- GW(Gateway) : 網關，外部核心網或伺服器


## 2.RTN(Return Link Network)
使用者 → 衛星 → 地面站 的上行路徑

(  UT → SAT  →  GW  )

此結構主要依賴 TDMA（時分多工） 或 MF-TDMA（多頻多時分多工） 機制，來在上行鏈路中有效分配時間與頻率資源。


## 3.Regenerator/RTN System

模擬上行鏈路的排程、資源分配、干擾與BER

## 4.MAC 層排程與接入控制

RTN 中最重要的部分之一是 MAC（Medium Access Control）機制。

為了應付多個終端共用同一顆衛星的頻寬，因此系統必須解決「誰在什麼時候可以傳」的問題。

- Demand Assignment (DA) : 終端先發送一個「資源需求」給網關，再由網關分配上行時槽。

- Dynamic Assignment：依據負載、QoS、延遲需求動態調整。

- Contention Resolution：若多個終端同時搶同一時槽，需偵測碰撞並重新傳送。

上述行為通常由 `SatRtnScheduler`/`SatRtnMac`/`SatSuperframeConf` 等類別控制

## 5.物理層（PHY）建模

上行鏈路(RTN)相較於下行鏈路(FL)，功率通常較弱、天線增益較低，因此 RTN 的 PHY 層特性更具挑戰性：
- 使用 上行頻段（如 Ka-band 29.5–30GHz）
- 調變與編碼方式（ModCod）：QPSK、8PSK、16QAM + LDPC
- 功率控制（Uplink Power Control）
- 天氣衰減（雨衰、雲霧吸收）
- 射頻干擾（Cross-polarization、Adjacent Beam Interference）
  
這些都會影響 EIRP、C/N0、BER、PER、Throughput 等效能指標。

此類行為，通常由`SatPhy`模組以及通道模型`SatChannel`實現

## 6. 時槽與超幀結構

RTN 的傳輸資源通常被組織成 超幀（Superframe），裡面包含：
- Random Access (RA) slots：用於初始接入或需求封包
- Assigned Slots：給已分配成功的終端
- Control Burst：控制與同步訊號

這樣的設計能確保多個終端公平使用上行頻寬。
在 SNS3 的設定檔中，會看到 `.conf` 或 `.xml` 檔描述這個結構。

## 7. 網路層與應用層的回傳效能

RTN 模擬的最終目的是觀察：
- 封包延遲（Round Trip Delay）
- 吞吐量（Throughput）
- 丟包率（Packet Loss）
- QoS 指標（不同流量類型的表現）

例如一個模擬場景可能是：
- 多個用戶端同時回傳影像資料（UDP流）
- 系統需分配 RTN 時槽給每個終端
- 分析在不同負載下系統的穩定性與效率

## 8.在 SNS3 模擬中的應用

當執行`./ns3 run sat-rtn-system-test-example`，
系統會模擬：

- 多個使用者終端的回傳封包
- MAC 層動態排程
- 衛星通道延遲與衰減
- 封包統計（延遲、吞吐、丟包）

可透過修改 `.ini` 或 `.conf` 文件改變：
- 用戶數量
- 帶寬
- ModCod 組合
- 排程策略

來觀察 RTN 效能變化













 



