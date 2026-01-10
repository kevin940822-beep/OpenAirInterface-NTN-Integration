### check virtual machine have python
```
python3 --version
```

### install python 
```
sudo apt update
sudo apt install -y python3-pip python3-matplotlib python3-numpy
```

### create a new script
```
nano plot_scatter.py
```

###
```
import matplotlib.pyplot as plt

# ====== 修改這裡 ======
filename = "stat-per-ut-fwd-app-throughput-scatter-2.txt"
# =====================

time = []
throughput = []

with open(filename, "r") as f:
    for line in f:
        line = line.strip()
        # 跳過註解與空行
        if not line or line.startswith("%") or line.startswith("#"):
            continue

        parts = line.split()
        if len(parts) != 2:
            continue

        t, th = parts
        time.append(float(t))
        throughput.append(float(th))

# 畫 scatter plot
plt.figure()
plt.scatter(time, throughput, s=10)
plt.xlabel("Time (s)")
plt.ylabel("Throughput (kbps)")
plt.title(filename)
plt.grid(True)

plt.show()
```


