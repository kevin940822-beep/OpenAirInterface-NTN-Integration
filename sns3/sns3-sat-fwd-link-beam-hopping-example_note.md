# sns3 sat-fwd-link-beam-hopping-example.cc
> Refrence
> https://github.com/sns3/sns3-satellite/blob/master/examples/sat-fwd-link-beam-hopping-example.cc

## Step  
```
cd ~/workspace/bake/source/ns-3.43
./ns3 run "sat-fwd-link-beam-hopping-example --PrintHelp"
```
*We need to move to path ns-3.43 so that we can run the code*

*but the code path at*  
```
cd ~/workspace/bake/source/ns-3.43/contrib/satellite/examples
```

### Output : 
<img width="1157" height="468" alt="image" src="https://github.com/user-attachments/assets/bcbeaedf-bf25-484c-a1ea-dc26acec9511" />

| 參數名稱           | 功能說明                                                | 預設值        | 修改範例                               |
| -------------- | --------------------------------------------------- | ---------- | ---------------------------------- |
| `--simTime`    | **模擬時間長度（秒）**。決定整個衛星系統跑多久                           | `3`（秒）     | `--simTime=10`<br>（模擬 10 秒）        |
| `--scaleDown`  | 是否**縮小 Forward Link 的頻寬**，方便在低流量下觀察 beam hopping 差異 | `true`     | `--scaleDown=0`<br>(關閉縮放，使用原始較大頻寬) |
| `--OutputPath` | **輸出統計結果的資料夾路徑**                                    | 未指定（用預設路徑） | `--OutputPath=./bh-test1`            |

### 修改參數並執行
```
mkdir -p results/bh-test1
./ns3 run "sat-fwd-link-beam-hopping-example --simTime=3 --scaleDown=1 --OutputPath=results/bh-test1"
```
*At fIrst time runinng, we can use less beam, to ensure the system run correctly*

*Cause beam hopping will cost some time*

### Output

<img width="173" height="363" alt="image" src="https://github.com/user-attachments/assets/da14ab8f-f044-43a5-be4d-0741bf1080a1" />

### 檢視結果檔
```
cd ~/workspace/bake/source/ns-3.43/results/bh-test1
ls
```
### Output

<img width="993" height="550" alt="image" src="https://github.com/user-attachments/assets/5c5fc1f7-6271-48fc-8a29-dafdb01758a5" />

### 重要檔案
### （A）Global（整個系統，只有 1 組）
| 檔案名稱                                                  | 層級     | 指標             | 實際作用                                       |
| ----------------------------------------------------- | ------ | -------------- | ------------------------------------------ |
| `stat-global-fwd-app-throughput-scalar.txt`           | Global | App Throughput | **整個系統的平均 Forward Link 吞吐量**               |
| `stat-global-fwd-app-throughput-scatter-0.txt`        | Global | App Throughput | 隨時間變化的吞吐量                             |
| `stat-global-fwd-app-delay-cdf-0-ATTN.txt`            | Global | App Delay      | **Forward link end-to-end delay 的 CDF 分佈** |
| `stat-global-fwd-composite-sinr-cdf-0-ATTN.txt`       | Global | **SINR**           | **整體 Forward link 的 SINR 分佈**              |
| `stat-global-fwd-feeder-mac-throughput-scalar.txt`    | Global | MAC Throughput | Feeder link MAC 層的平均吞吐量                    |
| `stat-global-fwd-feeder-mac-throughput-scatter-0.txt` | Global | MAC Throughput | Feeder link MAC 隨時間變化的吞吐量                    |
| `stat-global-fwd-user-mac-throughput-scalar.txt`      | Global | MAC Throughput | User link MAC 層平均吞吐量                       |
| `stat-global-fwd-user-mac-throughput-scatter-0.txt`   | Global | MAC Throughput | User link MAC 隨時間變化的吞吐量                       |

## Feeder link v.s. User link

| 項目              | Feeder link MAC                | User link MAC             |
| --------------- | ------------------------------ | ------------------------- |
| 位置              | Gateway ↔ Satellite            | Satellite ↔ UT            |
| 層級              | 核心 / 回程                        | 使用者存取                     |
| 服務對象            | 多個 beam / 多個 UT 的總計流量 | 單一 UT                     |
| 頻寬              | 通常較大                           | 較小、共享                     |
| Link quality    | 穩定                             | 易受 **SINR** / beam hopping 影響 |
| 是否容易成為瓶頸        | ❌ 較少                           | ✅ **非常容易**                |
| Beam hopping 影響 | 間接                             | **直接影響**                  |
| 對應檔案          | `fwd-feeder-mac-*`             | `fwd-user-mac-*`          |

### 查看feeder mac throughput 
```
less stat-global-fwd-feeder-mac-throughput-scalar.txt
```
### Output
<img width="502" height="111" alt="image" src="https://github.com/user-attachments/assets/67950241-7899-46b0-8ab1-53af5e790fb4" />

### 查看user mac throughput
```
less stat-global-fwd-user-mac-throughput-scalar.txt
```
### Output
<img width="502" height="111" alt="image" src="https://github.com/user-attachments/assets/34c14398-1918-44df-8be6-8470f9f12003" />

### 為甚麼feederlink沒有資料？
這個範例的重點是：**FWD User Link 的 Beam Hopping**

不是 end-to-end（GW → SAT → UT）完整傳輸。

所以Application 流量只跑在 **User link（Satellite → UT）** `1.00848e+06 kps`(大約=1Gbps)

**User link 才是真正受 beam hopping 影響**的部分，並計算：
- Throughput
- Delay
- SINR
等數據。

## SINR（Signal to Interference plus Noise Ratio）
**接收到的有用訊號功率，相對於干擾＋雜訊的比例**

<img width="301" height="74" alt="image" src="https://github.com/user-attachments/assets/a76f3120-13fe-42ce-b240-8f427f9d09fe" />

通常用dB表示

| 符號                      | 意義                 |
| --- | --- |
| Psingal      | beam 訊號來源   |
| Pinterference | 其他 beam、其他使用者造成的干擾 |
| Pnoise       | 熱雜訊、接收器雜訊          |

**SINR = 實體層品質的總結指標**
​​
- **SINR**越高：連線品質好 → throughput 高、delay 低
- **SINR**越低：需要多重船 → throughput 掉、delay 高

**Beam Hopping可以改善環境 → 改變 SINR 分佈**
- 同一時間只有部分 beam 被點亮
- 收到的訊號S ↑
- 受到其他beam的干擾 ↓

### 對應程式碼
```
stat-global-fwd-composite-sinr-cdf-0-ATTN.txt
```
### Output
<img width="500" height="600" alt="image" src="https://github.com/user-attachments/assets/d36b2eb2-797d-403f-add4-d5f50750a2a4" />

| 欄位 | 意義 |
| ---| ---|
| `OUTPUT_TYPE_CUMULATIVE` | 這是 **CDF（累積分佈）**|
| `mean`     | 平均 SINR ≈ **9.65 dB** |
| `stddev`   | 標準差 ≈ 1.44 dB         |
| `variance` | 變異數                   |
| `% percentile_5`  | 最差 5% 的 SINR ≈ **8.28 dB**  |
| `% percentile_50%` | 中位數 ≈ **9.87 dB**           |
| `% percentile_95%` | 最好 5% 的 SINR ≈ **11.13 dB** |









## What is Beam Hopping?

| 項目        | Static Beam | Beam Hopping |
| --------- | ----------- | ------------ |
| Beam 是否常亮 | 是           | 否（輪流）        |
| 頻寬分配      | 固定          | 動態           |
| 適合流量      | 平均          | 不平均          |
| 系統彈性      | 低           | 高            |
| 控制複雜度     | 低           | 高            |
| 模擬時間      | 快           | 較慢           |
