# SNS3 sat-rtn-system-test-example.cc
>### Refrence 
>https://github.com/sns3/sns3-satellite/blob/0fc2b8c74f0d9c2b0c3ee4ed132064a40ad2daf1/examples/sat-rtn-system-test-example.cc

## Step
### 檢視可修改的參數
```
./ns3 run sat-rtn-system-test-example -- --PrintHelp
```


<img width="737" height="498" alt="image" src="https://github.com/user-attachments/assets/fb96d95c-03d9-434d-a415-6dc8961e3ae9" />

- ```--testCase```：選擇要跑哪一個內建測試案例(通常不改)。預設為```--testCase=0```
- ```--frameConf```：指定 Superframe / Frame 結構配置(RTN slot 數量\頻寬配置\資源分配方式)。```--frameConf=Configuration_1```
- ```--trafficModel```：決定 RTN 回傳資料的流量型態。[0 = CBR (Constant Bit Rate)，1 = OnOff](https://github.com/kevin940822-beep/OpenAirInterface-NTN-Integration/edit/main/sns3/sns3-sat-rtn-system-test_note.md#10cbr-vs-onoff)。p


創建新資料夾給測試(方便之後修改使用)
```
mkdir -p results/rtn-test1
```

修改參數
```
./ns3 run sat-rtn-system-test-example -- --simLength=120 --trafficModel=1 --frameConf=Configuration_1 --beamId=10 --utAppStartTime=+1s --OutputPath=results/rtn-test1
```

### Result :

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/c60f707a-41df-4b89-bdd8-7bb5311ae607" />
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/bbdf4036-20f3-46c3-a961-63946d666987" />

```
115 Bytes allocated within TBTP
130 Bytes allocated within TBTP
144 Bytes allocated within TBTP
```
### 左圖為第一次執行時編譯器的輸出結果，右圖為執行結果
- 右圖為 RTN（Return Link）排程器在分配回傳時槽（TBTP）時的輸出訊息。
- 每一行的數字（115、130、144 Bytes）代表在該傳輸時槽中分配出去的 payload 大小。
- 這些訊息持續出現，表示排程器正在運作、衛星網路模擬正在「傳送回傳資料流」。

換句話說：
模擬器正在模擬多個終端的回傳封包，TBTP 正在持續被配置。

這是 `sat-rtn-system-test-example.cc` 的主要功能之一。

執行結束後，可以輸入此指令查看輸出檔（CSV / JSON）。

```
cd ~/workspace/bake/source/ns-3.43
ls contrib/satellite/data/sims
ls contrib/satellite/data/sims/example-rtn-system-test
```
會跑出
```
results.csv
trace.json
log.txt
```
這些就是模擬結果

---

# Table of Contents
1. UT/SAT/GW
2. RTN 上行/回傳鏈路
3. Regenerator/RTN System
4. MAC 層排程與接入控制
5. 物理層（PHY）建模
6. 時槽與超幀結構
7. 網路層與應用層的回傳效能
8. 在 SNS3 模擬中的應用
9. ACM 影響
10. CBR vs OnOff
11. 程式結果檔
12. architecture diagram(架構圖)
13. flowchart (流程圖)
14. MSC (Message Sequence Chart)訊息序列圖

---
## 1.UT/SAT/GW
- UT(User Terminal) : 使用者終端

- SAT(Satellite Network) : 衛星

- GW(Gateway) : 地面端，外部核心網或伺服器


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

## 9.ACM 影響

（`--"ns3::SatWaveformConf::AcmEnabled=true" `等）：
通道好時用高階調變（每符號更多 bit），`Per-beam Throughput` 上升；通道差時退階以保 BER，延遲可能增加。

## 10.CBR vs OnOff

| 項目         | **CBR (Constant Bit Rate)** | **OnOff (On–Off Model)**  |
| :--------- | :-------------------------- | :------------------------ |
| **中文名稱**   | 固定速率流量模型                   | 開關式流量模型                             |
| **流量型態**   | 連續、穩定輸出                     | 週期性啟動與停止傳輸                        |
| **傳送特性**   | 傳輸速率固定，不會改變              | 「On」時以指定速率傳輸，「Off」時暫停傳輸     |
| **資料封包間隔** | 固定間隔時間                     | 取決於On與Off時間分佈（通常為常數或隨機）     |
| **適用場景**   | 模擬穩定串流、VoIP、影片播放        | 模擬突發性流量，如HTTP、IoT事件、感測器資料  |
| **流量穩定性**  | 高（流量均勻）                     | 低（具間歇性與突發性）                     |
| **模擬真實度**  | 低(理想化)                        |高(符合實際突發流量)                        |

### 主要參數比較

| 參數           | **CBR**             | **OnOff**             |
| :----------- | :------------------ | :-------------------- |
| `DataRate`   | 固定傳輸速率（如 "1Mbps"）   | On階段時的傳輸速率（如 "2Mbps"）     |
| `PacketSize` | 固定封包大小（如 512 bytes） | 與CBR相同，可設定固定封包大小        |
| `Interval`   | 封包間隔（如 0.005s）      | 由On/Off時間共同決定實際平均速率       |
| `OnTime`     | 無                       | 傳輸啟動的持續時間，可為常數或隨機分布   |
| `OffTime`    | 無                       | 傳輸暫停的時間，可為常數或隨機分布       |


  
## 11.程式結果檔

以下為幾個較為重要的程式結果
- `Per-beam RTN App Throughput`（每波束的應用層上行吞吐）：有散點圖與純量檔，拿來比不同 beam 的負載/效率。
- `Feeder Dev/MAC/PHY Throughput`（回傳饋線段在不同層的吞吐）：看瓶頸在哪一層（裝置、MAC、PHY）。
- `Average UT User RTN App Throughput` / `Delay`（CDF）：看「每個使用者平均」的上行吞吐與延遲分佈（服務公平性、尾端延遲）。
- `Per-beam RTN App Delay`（CDF）：比較不同 beam 的延遲差異。
- `Per-beam Frame Symbol Load` / `Waveform Usage`：看每個超影格的符號負載與波形使用率（資源是否滿載、用到哪些 MODCOD/波形）。
- `Per-beam Feeder DA Packet Error`：資料承載服務的封包錯誤統計（看錯誤率是否升高）。

## 12.architecture diagram(架構圖)
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/4fd8fe41-7a33-4aee-9d91-566dcfab7894" />

## 13.flowchart(流程圖)

## 14.MSC (Message Sequence Chart)(訊息序列圖)
<img width="1099" height="519" alt="image" src="https://github.com/user-attachments/assets/7c1e5cab-2cf8-42d8-a7ec-10f8d271ab68" />






 



