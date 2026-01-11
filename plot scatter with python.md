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
#!/usr/bin/env python3
import argparse
import glob
import os
import re
import sys
from pathlib import Path

import matplotlib.pyplot as plt


def natural_key(s: str):
    # "file-2" < "file-10"
    return [int(t) if t.isdigit() else t.lower() for t in re.split(r"(\d+)", s)]


def iter_files(inputs, recursive: bool):
    """
    inputs can be:
      - file path
      - directory
      - glob pattern (wildcard)
    """
    out = []
    for x in inputs:
        p = Path(x)

        # directory -> default pattern inside
        if p.exists() and p.is_dir():
            pat = str(p / "**" / "*.txt") if recursive else str(p / "*.txt")
            out.extend(glob.glob(pat, recursive=recursive))
            continue

        # exact file
        if p.exists() and p.is_file():
            out.append(str(p))
            continue

        # treat as glob
        out.extend(glob.glob(x, recursive=recursive))

    # unique + natural sort
    out = sorted(set(out), key=natural_key)
    return out


def parse_two_columns(path: str, xcol: int, ycol: int, delimiter: str | None):
    xs, ys = [], []
    with open(path, "r", encoding="utf-8", errors="ignore") as f:
        for line in f:
            line = line.strip()
            if not line or line.startswith("%") or line.startswith("#"):
                continue

            # split by delimiter if user specifies, else split by whitespace or comma
            if delimiter:
                parts = line.split(delimiter)
            else:
                # allow commas or whitespace
                parts = re.split(r"[,\s]+", line)

            # skip malformed lines
            if len(parts) <= max(xcol, ycol):
                continue

            try:
                xs.append(float(parts[xcol]))
                ys.append(float(parts[ycol]))
            except ValueError:
                # skip non-numeric rows
                continue
    return xs, ys


def choose_file(files, auto_index: int | None):
    if auto_index is not None:
        if auto_index < 0 or auto_index >= len(files):
            raise SystemExit(f"--index out of range: {auto_index} (0..{len(files)-1})")
        return files[auto_index]

    for i, f in enumerate(files):
        print(f"[{i}] {f}")
    while True:
        s = input("Choose index: ").strip()
        if not s:
            continue
        try:
            idx = int(s)
            if 0 <= idx < len(files):
                return files[idx]
        except ValueError:
            pass
        print("Invalid index. Try again.")


def main():
    ap = argparse.ArgumentParser(
        description="Universal scatter plotter for ns-3 style *scatter*.txt files."
    )
    ap.add_argument(
        "inputs",
        nargs="*",
        default=["stat-*-scatter-*.txt"],
        help="File(s), directory(ies), or glob pattern(s). Default: stat-*-scatter-*.txt",
    )
    ap.add_argument("--recursive", "-r", action="store_true", help="Recursive search in directories / glob **")
    ap.add_argument("--index", "-i", type=int, default=None, help="Auto choose file by index (skip prompt)")
    ap.add_argument("--xcol", type=int, default=0, help="0-based column index for X (default: 0)")
    ap.add_argument("--ycol", type=int, default=1, help="0-based column index for Y (default: 1)")
    ap.add_argument("--delimiter", default=None, help="Delimiter, e.g. ',' or '\\t'. Default: auto (comma/space)")
    ap.add_argument("--s", type=float, default=10, help="Marker size (default: 10)")
    ap.add_argument("--title", default=None, help="Plot title (default: filename)")
    ap.add_argument("--xlabel", default="Time (s)", help="X axis label")
    ap.add_argument("--ylabel", default="Value", help="Y axis label")
    ap.add_argument("--no-grid", action="store_true", help="Disable grid")
    ap.add_argument("--save", "-o", default=None, help="Save figure to PNG/PDF/SVG (e.g. out.png)")
    ap.add_argument("--show", action="store_true", help="Force show window (default shows unless --save-only)")
    ap.add_argument("--save-only", action="store_true", help="Only save, do not open window")
    args = ap.parse_args()

    files = iter_files(args.inputs, recursive=args.recursive)
    if not files:
        raise SystemExit(f"No files match inputs: {args.inputs}")

    filename = choose_file(files, args.index)
    print("Using:", filename)

    xs, ys = parse_two_columns(filename, args.xcol, args.ycol, args.delimiter)
    if not xs:
        raise SystemExit("No numeric data parsed. Check columns/delimiter or file format.")

    plt.figure()
    plt.scatter(xs, ys, s=args.s)
    plt.xlabel(args.xlabel)
    plt.ylabel(args.ylabel)
    plt.title(args.title if args.title else os.path.basename(filename))
    if not args.no_grid:
        plt.grid(True)

    if args.save:
        out = Path(args.save)
        out.parent.mkdir(parents=True, exist_ok=True)
        plt.tight_layout()
        plt.savefig(out)
        print("Saved:", out)

    # default behavior: show unless save-only
    if args.show or (not args.save_only):
        plt.show()


if __name__ == "__main__":
    main()


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
