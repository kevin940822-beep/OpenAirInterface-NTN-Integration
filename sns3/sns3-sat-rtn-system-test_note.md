# SNS3 sat-rtn-system-test-example.cc
>### Refrence 
>https://github.com/sns3/sns3-satellite/blob/0fc2b8c74f0d9c2b0c3ee4ed132064a40ad2daf1/examples/sat-rtn-system-test-example.cc

### sns3 set up
>https://github.com/kevin940822-beep/OpenAirInterface-NTN-Integration/blob/main/sns3/SNS3_installation.md#sns3-installation

## Step
### 檢視可修改的參數
```
./ns3 run sat-rtn-system-test-example -- --PrintHelp
```


<img width="350" height="350" alt="image" src="https://github.com/user-attachments/assets/fb96d95c-03d9-434d-a415-6dc8961e3ae9" />

| 參數 | 說明 | 預設值 | 修改範例 |
|---|---|---|---|
| `--testCase` | 選擇要跑哪一個內建測試案例（通常不改） | `--testCase=0` | `--testCase=0` |
| `--frameConf` | 指定 Superframe / Frame 結構配置（RTN slot 數量、頻寬配置、資源分配方式） | `Configuration_0` | `--frameConf=Configuration_1` |
| `--trafficModel` | 決定 RTN 回傳資料的流量型態。[0 = CBR (Constant Bit Rate)，1 = OnOff](https://github.com/kevin940822-beep/OpenAirInterface-NTN-Integration/blob/main/sns3/sns3-sat-rtn-system-test_note.md#10cbr-vs-onoff) | `0` | `--trafficModel=1` |
| `--simLength` | 模擬時間（秒） | `30` | `--simLength=120` |
| `--beamId` | 指定使用的衛星 Beam（波束） | `26` | `--beamId=10` |
| `--utAppStartTime`   | UT（User Terminal）Application 什麼時候開始傳資料 | `+100ms` | `--utAppStartTime=+1s` |
| `--OutputPath` | 指定輸出統計檔案的資料夾 | （未指定） | `mkdir -p results/rtn-exp1`<br>`./ns3 run sat-rtn-system-test-example -- \`<br>`--OutputPath=results/rtn-exp1` |
| `--InputXml` | 指定 XML 設定檔（節點數、Frame 結構、FWD / RTN 參數） | `contrib/satellite/examples/sys-rtn-test.xml` | `--InputXml=contrib/satellite/examples/sys-rtn-test.xml` |



### 創建新資料夾給測試結果(方便之後修改使用)
```
mkdir -p results/rtn-test1
```

### 修改參數
```
./ns3 run sat-rtn-system-test-example -- \
--simLength=120 \
--trafficModel=1 \
--frameConf=Configuration_1 \
--beamId=10 \
--utAppStartTime=+1s \
--OutputPath=results/rtn-test1
```
### 前往資料夾位置
```
cd /home/kevin/workspace/bake/source/ns-3.43/results/rtn-test1
ls -la
```
### output

<img width="736" height="608" alt="image" src="https://github.com/user-attachments/assets/9c39e4ca-4f40-44a4-b851-5002ca77654d" />

### 重要檔案

| 類別 | 指標名稱 | 說明 | 對應輸出檔案 |
|---|---|---|---|
| **App Throughput** | Global App Throughput (Scalar) | 整體 RTN 應用層吞吐量的單一平均數值，用來比較不同參數設定下的整體效能 | `stat-global-rtn-app-throughput-scalar.txt` |
|  | App Throughput (Scatter) | RTN 應用層吞吐量隨時間變化的數據，常用來畫 Throughput vs. Time 圖，觀察流量穩定度與突波 | `stat-global-rtn-app-throughput-scatter-0.txt` |
| | Average UT User App Throughput (CDF) | 各使用者平均 RTN 應用層吞吐量的 CDF，用於分析使用者間的公平性（fairness） | `stat-average-ut-user-rtn-app-throughput-cdf.txt` |
| **Delay** | Average UT User App Delay (CDF) | RTN 應用層延遲的 CDF，用於評估 RTN 時延表現與整體效能分析 | `stat-average-ut-user-rtn-app-delay-cdf-ATTN.txt` |
| **MAC Throughput** | RTN User MAC Throughput (Scalar) | RTN 使用者端 MAC layer 的吞吐量，反映無線資源在使用者端的實際利用情況 | `stat-global-rtn-user-mac-throughput-scalar.txt` |
| | RTN Feeder MAC Throughput (Scalar) | RTN feeder link MAC layer 的吞吐量，用來分析核心鏈路的負載與瓶頸 | `stat-global-rtn-feeder-mac-throughput-scalar.txt` |
| **Beam / Frame 使用率** | Frame Symbol Load per Beam | 各個衛星 beam 的 frame symbol 使用率，反映 RTN 資源分配與 beam 負載狀況 | `stat-per-beam-frame-symbol-load-scalar.txt` |

### 檢視檔案內容
| 檢視方式 | 指令格式 | 說明 | 使用範例 |
|---|---|---|---|
| 直接顯示內容 | `cat 檔名` | 一次顯示整個檔案內容，適合內容較少或只需要快速查看 | `cat stat-global-rtn-app-throughput-scalar.txt` |
| 分頁檢視 | `less 檔名` | 以分頁方式檢視檔案內容，適合內容較多的檔案 | `less stat-global-rtn-app-throughput-scatter-0.txt` |
| 檢視前/後幾行 | `head 檔名` / `tail 檔名` | 只顯示檔案的前幾行或後幾行，適合快速確認檔案格式或最新資料 | `head stat-average-ut-user-rtn-app-throughput-cdf.txt` |
### Example :
```
less stat-global-rtn-app-throughput-scatter-0.txt
```
### Output :

<img width="386" height="784" alt="image" src="https://github.com/user-attachments/assets/f0c034da-e30d-43f6-85fa-ea5e6d8fa9b5" />


### Result :

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/c60f707a-41df-4b89-bdd8-7bb5311ae607" />
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/bbdf4036-20f3-46c3-a961-63946d666987" />

```
115 Bytes allocated within TBTP
130 Bytes allocated within TBTP
144 Bytes allocated within TBTP
```
### 左圖為第一次執行時編譯器的輸出結果，右圖為執行結果
- 右圖為 RTN（Return Link）排程器在分配回傳時槽 **TBTP(Terminal Burst Time Plan)** 時的輸出訊息。
  - **TBTP(Terminal Burst Time Plan)** ：是由 Hub 所產生的一份 回傳鏈路（Return Link, RTN）排程表，用來集中管理各個 UT（User Terminal） 的上行傳輸行為。
  - 內容包含：
    - 哪一個 UT（User Terminal）
    - 在哪一個 superframe / frame
    - 被分配到哪一個 time slot
    - 可以使用多少 symbols / bursts 進行傳輸
  - 實際運作方式：
    - FWD（Forward Link）：由 Hub 將 TBTP 傳送給各個 UT，作為回傳鏈路的傳輸規則說明。
    - Return Link（RTN）：各 UT 依照 TBTP 所指定的時間與資源配置，在對應的 time slot 中回傳資料。
- 每一行的數字（115、130、144 Bytes）代表在該傳輸時槽中分配出去的 payload 大小。
- 這些訊息持續出現，表示排程器正在運作、衛星網路模擬正在「傳送回傳資料流」。



換句話說：
模擬器正在模擬多個終端的回傳封包，TBTP 正在持續被配置。

這是 `sat-rtn-system-test-example.cc` 的主要功能之一。

執行結束後，可以輸入此指令查看輸出檔。

```
cd ~/workspace/bake/source/ns-3.43
ls contrib/satellite/data/sims
ls contrib/satellite/data/sims/example-rtn-system-test
```
會跑出以下模擬結果

<img width="1116" height="795" alt="image" src="https://github.com/user-attachments/assets/d78ac5e4-4573-4fcb-bbe0-9f18b6163bc3" />



對應結果
| 檔名關鍵字             | 意義      | 結果檔數量            |
| ----------------- | ------- | ------------------ |
| `stat-global-*`   | 全系統統計   | 全部加總 → 只有 1 個      |
| `stat-per-ut-*`   | 每個 UT   | UT 有 N 個 → 檔案有 N 個 |
| `stat-per-beam-*` | 每個 Beam | Beam 數量決定          |
| `scatter-*`       | 時間序列    | 每個實體 × 時間          |

所以才會有10個UT結果檔，因為前面手動將UT設定為10個

---

# Table of Contents
1. UT/SAT/GW
2. RTN 上行/回傳鏈路
3. MAC 層排程與接入控制
4. 物理層（PHY）建模
5. 時槽與超幀結構
6. 網路層與應用層的回傳效能
7. 在 SNS3 模擬中的應用
8. ACM 影響
10. CBR vs OnOff
11. architecture diagram(架構圖)
12. flowchart (流程圖)
13. MSC (Message Sequence Chart)訊息序列圖

---
## 1.UT/SAT/GW
- **UT(User Terminal)** : 使用者終端
  - 產生應用資料（RTN 上行）
  - 依照 TBTP 指定的 time slot 傳送資料
    
- **SAT(Satellite Network)** : 衛星
  - 接收 UT 的 RTN 資料
  - 轉發至 GW
  - 將 GW 的 FWD 控制訊息轉送給 UT
  - **不做排程決策**
  - **只負責轉送（bent-pipe）**

- **GW(Gateway)** : 地面端，外部核心網或伺服器
  - 產生 **TBTP（Terminal Burst Time Plan）**
  - 決定：哪個 UT、什麼時間、用多少 RTN 資源

<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/d11aa0f2-4625-4f18-b054-d601db22585e" />



## 2.RTN(Return Link Network)
**主要傳輸資料(App Data)**

路徑：

使用者 → 衛星 → 地面站

(  UT →  SAT  →  GW  )

此結構主要依賴 **TDMA（Time Division Multiple Access）時分多工** 或 **MF-TDMA（Multi-Frequency Time Division Multiple Access）多頻多時分多工** 機制，來在上行鏈路中有效分配時間與頻率資源。
| 項目     | TDMA | MF-TDMA |
| ------ | ---- | ------- |
| 切分方式   | 只有時間 | 時間 + 頻率 |
| 同時傳輸   | ❌    | ✅       |
| 頻寬利用率  | 較低   | 較高      |
| 衛星 RTN | 少用   | **主流**  |
|圖片比較|<img width="640" height="178" alt="image" src="https://github.com/user-attachments/assets/d233a5c8-dded-4339-93ad-2eeebaaad95a" />|<img width="458" height="264" alt="image" src="https://github.com/user-attachments/assets/1ac51ea1-7af9-4bff-ad27-b6f46efce6a1" />|



## 3.MAC 層排程與接入控制

### 程式碼位置

`cd ~/workspace/bake/source/ns-3.43/contrib/satellite/model`

RTN 中最重要的部分之一是 **MAC（Medium Access Control）** 機制。

應付多個終端共用同一顆衛星的頻寬，因此系統必須解決「誰在什麼時候可以傳」的問題。

所以**MAC**為**排程與資源分配的核心**。

- **Demand Assignment (DA)** : **需求式配置**。UT先發送一個「資源需求」給GW，再由GW分配RTN slot。

- **Dynamic Assignment**：**動態配置**。GW依據負載、QoS、延遲需求動態調整分配結果。

- **Contention Resolution**：**競爭式存取**。若多個終端同時搶同一時槽，需偵測碰撞並重新傳送。

| 層級 | 角色 | 主要工作內容 |
|---|---|---|
| **(1) Superframe / MF-TDMA 資源結構層**<br>（決定可用的 time slot / frequency 資源格子） | **GW（主要決策者）** | - 定義／選擇 superframe configuration（如 `Configuration_0 / Configuration_1`）<br>- 依系統設計決定 RTN 資源格子大小（時間 × 頻率）<br>- 限制後續 MAC 排程可用的資源範圍 |
| | **UT（遵循者）** | - 接收 GW 下發的 frame / superframe 結構資訊<br>- 依此得知可使用的時間與頻率資源 |
| | **SAT（透明轉發）** | - 透明轉發模式下不建立、不調整 superframe，只負責轉送 |
| **(2) 需求回報 + 動態分配層**<br>（DA / Dynamic Assignment）<br>（解決誰需要多少資源、誰先拿到） | **UT（提出需求）** | - 有資料要傳時產生容量需求（capacity request）(Random Access) <br>- 透過 RTN 控制訊息回報上行資源需求給 GW |
| | **GW（分配與排程）** | - 收集各 UT 的需求資訊<br>- 依負載、QoS、延遲需求等進行 Dynamic Assignment<br>- 形成類 TBTP 的資源分配結果 |
| | **SAT（透明轉發）** | - 不進行排程決策<br>- 僅轉送需求與控制訊息 |
| **(3) MAC 傳輸執行層**<br>（依排程真正送資料） | **UT（執行上行傳輸）** | - 依 GW 下發的排程規則（TBTP ）<br>- 在指定 time slot / burst 中進行 RTN 上行傳輸（MF-TDMA）<br>- 管理本地 MAC queue / dequeue |
| | **GW（接收與統計）** | - 接收 RTN 上行資料<br>- 負責 throughput / delay / load 等效能統計（如 `stat-global-*`、`feeder-mac-*`） |
| | **SAT（透明轉發）** | - 僅做訊號轉發（bent-pipe）<br>- 不進行 MAC 排程與 queue 控制|



| 功能                                | SNS-3 對應程式                                      |
| -------------------------------------- | ------------------------------------------------- |
| RTN MAC                                | `satellite-ut-mac.cc`                             |
| Demand Assignment / Dynamic Assignment | `satellite-llc.cc`                                |
| TBTP（排程結果）                             | `satellite-llc.cc` + `satellite-mac.cc`           |
| Superframe / MF-TDMA                   | `satellite-enums.h` + `satellite-*-net-device.cc` |



## 4.物理層（PHY）建模

### 程式碼位置

`cd ~/workspace/bake/source/ns-3.43/contrib/satellite/model`

上行鏈路(RTN)相較於下行鏈路(FL)，功率通常較弱、天線增益較低，因此 RTN 的 PHY 層特性更具挑戰性：
- 使用 上行頻段（如 Ka-band 29.5–30GHz）
- 調變與編碼方式（ModCod）：QPSK、8PSK、16QAM + LDPC
- 功率控制（Uplink Power Control）
- 天氣衰減（雨衰、雲霧吸收）
- 射頻干擾（Cross-polarization、Adjacent Beam Interference）
  
這些都會影響 EIRP、C/N0、BER、PER、Throughput 等效能指標。

此類行為主要由 **衛星 PHY 層模組**`satellite-phy-*`與 **通道與傳播模型**`satellite-channel`/ `propagation loss models` 所實現。

| 模組 | 主要負責內容 | 相關程式 |
|---|---|---|
| **PHY（Physical Layer）** | 調變編碼、功率處理<br>接收品質評估、封包成功判斷 |`satellite-phy-tx.cc`<br>`satellite-phy-rx.cc`<br>`satellite-phy-rx-carrier-uplink.cc`<br>`satellite-phy-rx-carrier-downlink.cc`  |
| **Channel（通道模型）** | 路徑損耗、雨衰<br>雜訊、干擾、訊號疊加 | `satellite-channel.cc`<br>`satellite-propagation-loss-model.cc` |

  
## 5. 時槽與超幀結構（Superframe）

Superframe 的主要目的在於：
- 將 RTN 的上行頻寬切分為結構化的時間與資源單位
- 作為 MAC 排程（TBTP / DA / Dynamic Assignment） 的基礎
  
RTN 的傳輸資源通常被組織成 超幀（Superframe），裡面包含：
- Random Access (RA) slots：
  - 容量需求回報（capacity request）
  - 初始接入（initial access）
- Assigned Slots：給已分配成功的終端
- Control Burst：控制與同步訊號

這樣的設計能確保多個終端公平使用上行頻寬。
在 SNS3 的設定檔中，會看到 `.conf` 或 `.xml` 檔描述這個結構。

## 6. 網路層與應用層的回傳效能

RTN 模擬的最終目的是觀察：
- 封包延遲（Round Trip Delay）
- 吞吐量（Throughput）
- 丟包率（Packet Loss）
- QoS 指標（不同流量類型的表現）

例如一個模擬場景可能是：
- 多個用戶端同時回傳影像資料（UDP流）
- 系統需分配 RTN 時槽給每個終端
- 分析在不同負載下系統的穩定性與效率

## 7.在 SNS3 模擬中的應用

當執行`./ns3 run sat-rtn-system-test-example`，
系統會模擬：

- 多個使用者終端的回傳封包
- MAC 層動態排程
- 衛星通道延遲與衰減
- 封包統計（延遲、吞吐、丟包）

可透過修改參數改變：
- 用戶數量
- 帶寬
- ModCod 組合
- 排程策略

來觀察 RTN 效能變化

## 8.ACM 影響
### 程式位置

`cd ~/workspace/bake/source/ns-3.43/contrib/satellite/model/`
```
satellite-waveform-conf.cc
satellite-waveform-conf.h
```
ACM 的實作主要位於 SNS-3 的 waveform 與 PHY 模組

`satellite-phy-rx-*.cc`

`satellite-waveform-conf.cc`


可透過設定
`--ns3::SatWaveformConf::AcmEnabled=true `啟用自適應調變與編碼（ACM）機制。

ACM 由 PHY 層根據即時通道品質（如 C/N₀）自動選擇適當的調變與編碼方式（ModCod）：

- **通道條件良好時**：  
  採用較高階調變（如 8PSK、16QAM），每個 symbol 可承載更多位元，使 **per-beam throughput 提升**。
- **通道條件惡化時**：  
  退回較低階調變（如 QPSK）以維持可接受的 BER / PER，但可能導致 **傳輸效率下降與延遲增加**。

<img width="224" height="626" alt="image" src="https://github.com/user-attachments/assets/472f2539-4f56-460e-8723-1650f4b68a99" />

## 9.CBR vs OnOff

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


## 10.architecture diagram(架構圖)
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/4fd8fe41-7a33-4aee-9d91-566dcfab7894" />

## 11.flowchart(流程圖)

## 12.MSC (Message Sequence Chart)(訊息序列圖)
<img width="1099" height="519" alt="image" src="https://github.com/user-attachments/assets/7c1e5cab-2cf8-42d8-a7ec-10f8d271ab68" />






 



