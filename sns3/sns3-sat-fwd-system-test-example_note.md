Refrence : https://github.com/sns3/sns3-satellite/blob/master/examples/sat-fwd-system-test-example.cc

# Table of Contents 
- [Table of Contents](#table-of-contents)
- [Step](#step)


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

## BBFrame Tx
`PrintBbFrameInfo()` 會在每次 BBFrame 傳送時，印出：
- 時間
- Frame type
- ModCod
- Occupancy、Duration
- Space used/left
- 以及 BBFrame 裡每個 payload packet 的 接收端地址（DestAddress）

> line 44-84

### Output
<img width="1845" height="106" alt="image" src="https://github.com/user-attachments/assets/6f7f70a5-10b4-441e-b096-d96f0b5f63f9" />

## BBFrame Merge
`PrintBbFrameMergeInfo()` 會印出 merge 的起訖，再呼叫 `PrintBbFrameInfo()` 分別印 mergeTo / mergeFrom。

> Line 89-98

### Output
