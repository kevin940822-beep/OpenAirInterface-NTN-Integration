Refrence : https://github.com/sns3/sns3-satellite/blob/master/examples/sat-fwd-system-test-example.cc

# Table of Contents 
- [Table of Contents](#table-of-contents)
- [Step](#step)
- [BBframe](#bbframe)
  - [BBHEADER](#bbheader)
  - [BBframe Tx](#bbframe-tx)
  - [BBFrame Merge](#bbframe-merge)


## Step 
```
cd ~/workspace/bake/source/ns-3.43
./ns3 run "sat-fwd-system-test-example.cc --PrintHelp"
```
### Output
<img width="1038" height="251" alt="image" src="https://github.com/user-attachments/assets/f6d5fd46-e9d3-444e-bf6f-0e05703ec906" />


| 參數名稱 | 預設值      | 參數用途說明 |
| ---| ---| ---|
| `--testCase`            | `0`| 指定要執行的測試案例（Test case）。<br>• `0`：啟用 scheduler、**ACM 關閉**<br>• `1`：啟用 scheduler、**ACM 開啟**<br>• `2`：**ACM 模式**，僅模擬 **單一 UT（User Terminal）** |
| `--gwEndUsers`         | `10`     | Gateway（GW）端所連接的 **終端使用者數量**。<br>在 FWD 中，代表 GW 同時向多少個使用者發送資料流量。                                 |
| `--simLength`          | `40`     | 模擬總時間長度（通常單位為 **秒**）。<br>控制整個 ns-3 模擬執行多久。                                                              |
| `--traceFrameInfo`     | `false`  | 是否輸出 **BBFRAME（Baseband Frame）詳細資訊**。<br>• `true`：列印每個 BB frame 的資訊（用於除錯與分析）<br>• `false`：不輸出      |
| `--traceMergeInfo`     | `false`  | 是否輸出 **BBFRAME 合併（merge）相關資訊**。<br>通常用於觀察多個流量如何被合併進同一個 BB frame。                                  |
| `--beamId`             | `26`     | 指定使用的 **Beam ID**。<br>代表 Forward Link 中 GW 對哪一個衛星波束（beam）進行傳輸。                                            |
| `--trafficModel`       | `cbr`    | 設定流量模型（Traffic Model）。<br>• `cbr`：固定速率流量（Constant Bit Rate）<br>• `onoff`：間歇式流量（On/Off）                   |
| `--senderAppStartTime` | `+100ms` | 傳送端應用程式的 **啟動時間**。<br>避免模擬一開始系統尚未穩定就立刻送資料。                                                        |

```
./ns3 run sat-fwd-system-test-example -- --testCase=0 --gwEndUsers=2 --simLength=4 --traceFrameInfo=false --traceMergeInfo=false --beamId=26 --trafficModel=cbr --senderAppStartTime=100ms 
```
### Output
<img width="212" height="263" alt="image" src="https://github.com/user-attachments/assets/63cf4d64-809c-4a32-860f-937feeec4e7a" />


---

## BBframe（BaseBand Frame）
**BBFrame（BaseBand Frame）**  : 
- 是來自 DVB-S2 / DVB-S2X 標準，屬於 **實體層（Physical Layer）** 中，基頻處理的資料單位。
- 所有上層封包（IP / MPEG / GSE）都必須先被封裝進 BBFrame，才能形成**PLFRAME** 經調變後送上衛星載波。
- 通道編碼（channel coding）之前的資料格式，用來承載上層輸入資料。

### BBFrame 結構
<img width="735" height="253" alt="image" src="https://github.com/user-attachments/assets/a96f9e97-2299-4a56-a429-6e4993734a22" />

> Refrence : [ETSI EN 302 307-1](https://www.etsi.org/deliver/etsi_en/302300_302399/30230701/01.04.01_20/en_30230701v010401a.pdf) 5.2

BBFrame 包含三個主要部分：
- [BBHEADER](#bbheader)（固定長度 80 bits）
- DATA FIELD（payload，可變長度，此模擬器預設為4050）
- Padding（若 payload 不足）
  - 用來將BBframe補到固定長度 Kbch bits，所以會發生`Occupancy < 1`。


## BBHEADER
<img width="889" height="279" alt="image" src="https://github.com/user-attachments/assets/c8dbf7af-308b-4f92-95be-87390d255076" />

>Refrence :
>
> [ETSI EN 302 307-1](https://www.etsi.org/deliver/etsi_en/302300_302399/30230701/01.04.01_20/en_30230701v010401a.pdf) 5.1.5
>
>[ETSI EN 302 307-1](https://www.etsi.org/deliver/etsi_en/302300_302399/30230701/01.04.01_20/en_30230701v010401a.pdf) 5.1.6

MATYPE（2 bytes）: BBFRAME 承載資料流的種類:

### First Byte (MATYPE-1)
| 欄位 | 位元數 | 設定值 | 功能說明|
| --- | ---| ---| ---|
| **TS / GS**   | 2 bits | **TS**       | **Transport Stream Input**：典型為 **MPEG-TS（188 bytes/packet）**，常用於廣播 / 電視傳輸。|
|               |        | **GS**       | **Generic Stream Input**：泛用資料流（封包化或連續位元流），常用於 **IP / 資料服務**。|
|               |        |              |讓接收端知道 DATA FIELD 裡的**內容格式**，接收端才能用正確方式切封包、找同步、解封裝。              |
| **SIS / MIS** | 1 bit  | **SIS**      | **Single Input Stream**：僅一條輸入流（單一 service / 單一 stream）。                                                       |
|               |        | **MIS**      | **Multiple Input Stream**：多條輸入流（多 service / 多 stream）共用同一 DVB-S2 系統。                                      |
| **CCM / ACM** | 1 bit  | **CCM**      | **Constant Coding and Modulation**：調變與編碼固定（MODCOD 固定）。                                                         |
|               |        | **ACM**      | **Adaptive Coding and Modulation**：可依通道狀況調整 MODCOD。                                         |
| **ISSYI**     | 1 bit  | 0 / 1        | **Input Stream Synchronization Indicator**：<br>ISSYI=1 時，會在 User Packets 後插入 **ISSY field**（見 Annex D）<br>表示是否啟用**Input Stream Synchronization（輸入流同步）** 機制。<br>讓接收端做 **時序/同步相關處理**(更精準的時間對齊、同步恢復)。|
| **NPD**       | 1 bit  | 0 / 1        | **Null-packet Deletion**：<br>在 **TS（MPEG-TS）** 系統中，NPD=1 表示送端會刪除 **Null packet（PID 0x1FFF）**，以節省衛星資源（ACM 情境常見）。<br> 用於提升頻譜效率，避免傳送**純填充用**的封包。      |
| **RO**        | 2 bits | 00 / 01 / 10 | **Roll-off factor (α)**：成形濾波（如 Root-Raised-Cosine）參數，影響訊號頻寬與頻譜形狀；DVB-S2 常見值如 **0.35 / 0.25 / 0.20**。<br>用於讓接收端知道傳輸用的頻譜成形參數，以便正確匹配接收濾波與解調。                 |

### Second Byte(MATYPE-2)
| MATYPE-1型態                     | MATYPE-2 內容                      | 說明                                        |
| ------------------------------ | -------------------------------- | ----------------------------------------- |
| **SIS（Single Input Stream）**   | Reserved(保留)                       單一輸入流時，MATYPE-2 保留不用。                     |
| **MIS（Multiple Input Stream）** | **ISI（Input Stream Identifier）** | 指示此 BBFRAME 屬於哪一條輸入流，讓接收端在多輸入流情境下正確分流與解碼。 |


---

`[BBFrameTx] Frame Type: NORMAL_FRAME, ModCod: QPSK_1_TO_2` 代表Scheduler : 

- 從 GW 的佇列中拿出多個 packet
- 依照 ModCod 計算可用空間
- 把 packet 塞進 BBFrame (盡量塞滿)
- 把這個 BBFrame 丟到 PHY 層傳送出去

<img width="316" height="376" alt="image" src="https://github.com/user-attachments/assets/fb03e5aa-b453-442b-b4ef-57c59c3a8ccc" />

`Frame Type: DUMMY_FRAME` : 代表此時還未有資料要送，但PHY 時序仍需維持

### BBFrame Tx
```
--traceFrameInfo=true --traceMergeInfo=false
```
`PrintBbFrameInfo()` 會在每次 BBFrame 傳送時，印出：
- 時間
- Frame type
- ModCod
- Occupancy (佔用的空間%)、Duration (固定`+3.19507e+06 ns`)
- Space used/left (固定4050)
- BBFrame 裡每個 payload packet 的 接收端地址（DestAddress）

BBframe Tx會將每一個封包所抵達的目的做一次紀錄，所以在Output會看到很多位址
> line 44-84

一個封包大小固定為128 Bytes

### Output
<img width="1845" height="106" alt="image" src="https://github.com/user-attachments/assets/6f7f70a5-10b4-441e-b096-d96f0b5f63f9" />




### BBFrame Merge
```
--traceFrameInfo=false --traceMergeInfo=true
```

`PrintBbFrameMergeInfo()` 會印出 merge 的起訖，再呼叫 `PrintBbFrameInfo()` 分別印 mergeTo / mergeFrom。

> Line 89-98

### Output
