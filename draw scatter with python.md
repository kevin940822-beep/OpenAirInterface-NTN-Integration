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
import glob
import sys
import matplotlib.pyplot as plt

pattern = sys.argv[1] if len(sys.argv) > 1 else "stat-*-scatter-*.txt"
files = sorted(glob.glob(pattern))

if not files:
    raise SystemExit(f"No files match pattern: {pattern}")

for i, f in enumerate(files):
    print(f"[{i}] {f}")

idx = int(input("Choose index: ").strip())
filename = files[idx]
print("Using:", filename)

t, y = [], []
with open(filename) as f:
    for line in f:
        line = line.strip()
        if not line or line.startswith("%") or line.startswith("#"):
            continue
        a, b = line.split()
        t.append(float(a))
        y.append(float(b))

plt.scatter(t, y, s=10)
plt.xlabel("Time (s)")
plt.ylabel("Value")
plt.title(filename)
plt.grid(True)
plt.show()

```

### 前往資料夾
```
cd ~/workspace/bake/source/ns-3.43/results/rtn-test1
```
### 執行程式檔
```
python3 plot_scatter.py "stat-per-ut-rtn-feeder-mac-throughput-scatter-*.txt"
```
### Output

<img width="741" height="283" alt="image" src="https://github.com/user-attachments/assets/cce9e89c-8d1c-4738-8287-db8dfc2d72f9" />

Chose index : 1

<img width="733" height="668" alt="image" src="https://github.com/user-attachments/assets/39c4af27-b74a-487e-bdef-8a7700be4cc8" />

or 
```
python3 plot_scatter.py "stat-per-ut-rtn-feeder-mac-throughput-scatter-10.txt"
```
### Output


<img width="730" height="723" alt="image" src="https://github.com/user-attachments/assets/cb5bb845-3f66-4404-9f35-6b37d07ca5f7" />
