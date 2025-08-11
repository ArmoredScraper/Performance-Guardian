# Performance-Guardian
I was motivated last week to make an app to always have up that monitors my cpu, i/o, memory....And wifi. Now, whenever I make and run things on my computer, I can monitor what its using in live time so I can terminate it before it freezes my computer.
So I wrote yesterday about feeling the code. But when I woke up this morning, I realized I'd make myself more valuable if I did my drills, studied, and leverage AI to build things. I spent quiet a while making this today and I'm dead tired so I'll just post  the files for the code tomorrow so people can download it and adjust it to their need. 


https://github.com/user-attachments/assets/30ab6047-4a25-41f0-9209-86f14fb29737
# Performance-Guardian
I was motivated last week to make an app to always have up that monitors my cpu, i/o, memory....And wifi. Now, whenever I make and run things on my computer, I can monitor what its using in live time so I can terminate it before it freezes my computer.
So I wrote yesterday about feeling the code. But when I woke up this morning, I realized I'd make myself more valuable if I did my drills, studied, and leverage AI to build things. I spent quiet a while making this today and I'm dead tired so I'll just post  the files for the code tomorrow so people can download it and adjust it to their need. 
[performance_guardian_v0.1.0_linux_aarch64.tar.gz](https://github.com/user-attachments/files/21724147/performance_guardian_v0.1.0_linux_aarch64.tar.gz)

---

## Features

- **Compact GUI (egui/eframe)**
  - Always-on-top, draggable & resizable
  - **Lock/Unlock** position + **Scale** slider
  - Stable tables (no row shuffling)

- **Processes**
  - Top CPU, memory (MiB), human labels (e.g., `firefox (web browser)`)
  - **TERM** button to stop a process

- **Connections**
  - Live `ss -tunap`: proto, state, local/remote, PID, process

- **Devices (LAN)**
  - From ARP cache (`ip neigh`) or **arp-scan** (optional, sudo)
  - IP, MAC, vendor, simple type guess (iPhone/Android/Roku/etc.)

- **I/O**
  - **Network** up/down per interface (kB/s) from `/proc/net/dev`
  - **Disk** read/write per device (MB/s) from `/proc/diskstats`
  - **Temps** from `/sys/class/thermal`, **Battery %** from `/sys/class/power_supply`

- **CLI mode**
  - JSON stream for CPU/MEM: `performance_guardian --interval 2 --json`

---

## Install

### One-liner (builds from this repo)
```bash
# requires Rust toolchain (https://rustup.rs)
cargo install --git https://github.com/ArmoredScraper/Performance-Guardian --locked --features sysinfo
This installs:

    pg_gui – GUI app

    performance_guardian – CLI sampler

From source
git clone https://github.com/ArmoredScraper/Performance-Guardian.git
cd Performance-Guardian
cargo build --release --features sysinfo
# binaries: target/release/{pg_gui,performance_guardian}
Optional dependencies

    ss (iproute2 – default on most distros)

    arp-scan for better LAN discovery:

sudo apt install -y arp-scan
# optional: allow arp-scan without password (understand the risk)
sudo setcap cap_net_raw,cap_net_admin+eip "$(command -v arp-scan)"
Usage
GUI

pg_gui        # or: ~/.cargo/bin/pg_gui

    Tabs: Processes · Connections · Devices · I/O

    Lock to freeze size/position; Scale to shrink/grow the HUD

    Wayland note: true browser “fullscreen” may cover the HUD; use maximized if you want it visible
performance_guardian --interval 2 --json | jq .
Autostart (optional)

sudo install -m0755 target/release/pg_gui /usr/local/bin/pg_gui
mkdir -p ~/.config/autostart
cat > ~/.config/autostart/performance-guardian.desktop <<'EOF'
[Desktop Entry]
Type=Application
Name=Performance Guardian
Exec=/usr/local/bin/pg_gui
X-GNOME-Autostart-enabled=true
EOF
How it works

    CPU/MEM via sysinfo (EMA smoothing, α=0.30)

    Processes refreshed every ~0.9s; top set chosen every 15s, then updated in place

    Connections via ss -tunap (sort to reduce jitter)

    Devices from ip neigh or arp-scan (if enabled)

    I/O diffs of /proc/net/dev and /proc/diskstats once per second

    Temps/Battery read from /sys/class/* every 5s

Performance tips

    Run release builds: cargo run --release …

    Keep the window small; lower the sample interval if you need less overhead

Troubleshooting

    No PIDs in Connections: run PG as root to see all sockets

    No temps/battery: some hardware exposes different sysfs paths

    HUD hidden by fullscreen: use maximized/tiling instead of F11 fullscreen

Roadmap

    Threshold alerts (CPU/MEM/IO/NET) with notifications

    System tray icon

    Per-process I/O (net/disk)

    Search & filters; export CSV/JSON; config file

License

MIT © 2025 Zachary Winn
MD

::contentReference[oaicite:0]{index=0}


