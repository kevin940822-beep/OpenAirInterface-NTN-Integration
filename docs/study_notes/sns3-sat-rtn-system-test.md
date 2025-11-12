# SNS3 sat-rtn-system-test-example.cc
>### Refrence 
>https://github.com/sns3/sns3-satellite/blob/0fc2b8c74f0d9c2b0c3ee4ed132064a40ad2daf1/examples/sat-rtn-system-test-example.cc
>
### Result :

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/c60f707a-41df-4b89-bdd8-7bb5311ae607" />

# Table of Contents
1.UT/SAT/GW
 
2.RTN 上行/回傳鏈路

3.Regenerator/RTN System

4.MAC 層排程與接入控制

5.物理層（PHY）建模


---
## 1.UT/SAT/GW
UT(User Terminal) : 使用者終端

SAT(Satellite Network) : 衛星

GW(Gateway) : 網關，外部核心網或伺服器


## 2.RTN(Return Link Network)
使用者 → 衛星 → 地面站 的上行路徑
(  UT → SAT  →  GW  )


## 3.Regenerator/RTN System

模擬上行鏈路的排程、資源分配、干擾與BER

## 4.MAC 層排程與接入控制

RTN 中最重要的部分之一是 MAC（Medium Access Control）機制。

為了應付多個終端共用同一顆衛星的頻寬，因此系統必須解決「誰在什麼時候可以傳」的問題。

> Demand Assignment (DA) : 終端先發送一個「資源需求」給網關，再由網關分配上行時槽。
>
> Dynamic Assignment：依據負載、QoS、延遲需求動態調整。
>
> Contention Resolution：若多個終端同時搶同一時槽，需偵測碰撞並重新傳送。

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






---

 



