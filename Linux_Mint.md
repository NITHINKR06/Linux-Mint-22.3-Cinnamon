# Linux Mint 22.3 Cinnamon — Complete Installation Guide
---

## ⚠️ CRITICAL WARNINGS — READ FIRST

- **NEVER** select "Erase disk and install Linux Mint" — this will wipe Windows too
- **NEVER** touch these Windows partitions:

- **ALWAYS** use "Something else" (manual partitioning)
- **NEVER** unplug USB during installation
- **NEVER** force shutdown during installation

---

## PART 1 — Create Bootable USB with Rufus

### What you need
- Linux Mint 22.3 ISO (already downloaded)
- 114 GB USB drive
- Rufus (download from rufus.ie)

### Steps

1. Plug in your USB drive
2. Open Rufus
3. Set these exact settings:
   - **Device** → select your 114 GB USB drive
   - **Boot selection** → click SELECT → choose your Linux Mint ISO file
   - **Partition scheme** → GPT ← important, your laptop is UEFI
   - **Target system** → UEFI (non CSM)
   - **File system** → FAT32
   - **Cluster size** → leave default
4. Click **START**
5. When asked "Write in ISO Image mode or DD Image mode?" → choose **ISO Image mode**
6. Click OK on the warning (this will erase the USB — that's fine)
7. Wait for it to finish — takes about 5-10 minutes
8. Click CLOSE when done

### ✅ Verify
- Rufus shows green "READY" at the bottom
- Do NOT eject USB yet — verify the ISO first

---

## PART 2 — Verify the ISO (Important)

This confirms your download is not corrupted or fake.

1. On the Linux Mint download page, right-click **sha256sum.txt** → Save Link As → save to Desktop
2. Open PowerShell and run:
```powershell
Get-FileHash C:\Users\nithin\Downloads\linuxmint-22.3-cinnamon-64bit.iso -Algorithm SHA256
```
3. Compare the output hash with the one in sha256sum.txt
4. They must match exactly — if not, re-download the ISO

---

## PART 3 — BIOS Settings Before Installing

1. **Shut down** Windows completely (not restart)
2. Power on and immediately press **F10** repeatedly (HP BIOS key)
3. In BIOS:
   - Go to **Security** tab → confirm **Secure Boot is OFF** (already off on your system ✅)
   - Go to **Boot Options** → enable **USB Boot**
   - Move USB drive to **top of boot order**
4. Press **F10** to save and exit
5. Laptop will restart and boot from USB

> **If F10 doesn't work** → try F9 for one-time boot menu, then select your USB

---

## PART 4 — Boot into Linux Mint Live

1. You'll see the **Linux Mint boot menu**
2. Select **Start Linux Mint 22.3 Cinnamon 64-bit** → press Enter
3. Wait 1-2 minutes — it loads a full desktop from USB (this is just a preview, nothing is installed yet)
4. You'll see a beautiful desktop with a DVD icon on it

### What you'll see — this is normal ✅
- Full Linux Mint desktop running from USB
- Internet may or may not work at this stage — doesn't matter

### Test before installing (optional but recommended)
- Check WiFi works
- Check keyboard and touchpad work
- Check screen brightness

---

## PART 5 — Installation (Most Critical Part)

### Start the installer
1. Double-click **"Install Linux Mint"** icon on the desktop
2. Select language → **English** → Continue

### Step by step screens

**Screen 1 — Keyboard layout**
- Select **English (US)** → Continue

**Screen 2 — Multimedia codecs**
- ✅ Check "Install multimedia codecs" → Continue
- (Needs internet, skip if no WiFi — you can install later)

**Screen 3 — Installation type ← MOST IMPORTANT SCREEN**
- You will see options like:
  - "Erase disk and install Linux Mint" ← **NEVER click this**
  - "Install alongside Windows" ← **DO NOT use this either**
  - "Something else" ← ✅ **SELECT THIS**
- Click **Something else** → Continue

---

## PART 6 — Manual Partitioning (Follow Exactly)

You will see a partition table. Here is what each partition is:

| Size | What it is | Action |
|------|-----------|--------|
| 170.18 GB | CachyOS root partition | ✅ Delete & reuse |
| 4.00 GB | CachyOS EFI partition | ✅ Delete & reuse |

### Delete CachyOS partitions

1. Click on the **170.18 GB** partition → click **Delete** (minus button)
   - It becomes "free space"
2. Click on the **4.00 GB** EFI partition → click **Delete**
   - It becomes "free space" too

### Create new Ubuntu/Mint partitions

**Create EFI partition:**
1. Click on the free space where 4 GB was → click **+** (Add)
2. Set:
   - Size: **512 MB**
   - Type: **Primary**
   - Use as: **EFI System Partition**
3. Click OK

**Create swap partition:**
1. Click remaining free space → click **+**
2. Set:
   - Size: **8192 MB** (8 GB = matches your RAM)
   - Type: **Primary**
   - Use as: **swap area**
3. Click OK

**Create root partition:**
1. Click remaining free space → click **+**
2. Set:
   - Size: **remaining all space** (leave as is)
   - Type: **Primary**
   - Use as: **ext4 journaling file system**
   - Mount point: **/**
3. Click OK

### Set bootloader location
- At the bottom: "Device for boot loader installation"
- Select the **main disk** (e.g., /dev/sda or /dev/nvme0n1) — NOT a specific partition
- This is important — installs GRUB which lets you choose between Linux Mint and Windows

### Click Install Now
- Review the summary popup carefully
- Confirm only the CachyOS partitions are being changed
- Click **Continue**

---

## PART 7 — Installation Process

1. **Where are you?** → Select your timezone → India (Kolkata) → Continue
2. **Who are you?**
   - Your name: enter your name
   - Computer name: e.g., `nithin-laptop`
   - Username: e.g., `nithin` (lowercase, no spaces)
   - Password: set a strong password — **remember this, you'll need it daily**
   - ✅ Select "Require password to log in"
3. Click **Continue**

### Wait for installation
- Takes **15-25 minutes**
- You'll see a slideshow — just wait
- Do NOT close the installer
- Do NOT shut down

---

## PART 8 — First Boot

1. When installation finishes → click **Restart Now**
2. You'll see: **"Please remove installation medium and press ENTER"**
3. Remove your USB drive → press Enter
4. Laptop restarts

### GRUB menu (boot chooser)
You'll see a black screen with options:
- **Linux Mint 22.3 Cinnamon** ← default, boots automatically after 10 seconds
- **Windows Boot Manager** ← select this to boot Windows 11

> If you don't see GRUB and Windows boots directly → see Troubleshooting section below

---

## PART 9 — First Login to Linux Mint

1. Enter the password you set during installation
2. Welcome screen will appear — go through it
3. Update Manager will prompt updates → **install all updates first thing**

---

## PART 10 — Install Your Dev Tools

Open Terminal (Ctrl+Alt+T) and run these in order:

### Install Go
```bash
wget https://go.dev/dl/go1.22.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.22.4.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
go version
```

### Install Node.js via nvm
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install --lts
node --version
```

### Install pnpm
```bash
corepack enable
corepack prepare pnpm@latest --activate
pnpm --version
```

### Install Docker
```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
newgrp docker
docker --version
```

### Install just (command runner)
```bash
curl --proto '=https' --tlsv1.2 -sSf https://just.systems/install.sh | bash -s -- --to ~/.local/bin
echo 'export PATH=$PATH:~/.local/bin' >> ~/.bashrc
source ~/.bashrc
just --version
```

### Install Go tools
```bash
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
go install gotest.tools/gotestsum@latest
go install golang.org/x/tools/cmd/goimports@latest
go install github.com/swaggo/swag/cmd/swag@latest
go install golang.org/x/vuln/cmd/govulncheck@latest
```

### Install Trivy
```bash
sudo apt-get install wget apt-transport-https gnupg -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb jammy main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```

---

## Troubleshooting

### GRUB not showing, boots straight to Windows
```bash
# Boot from USB again, open terminal and run:
sudo apt install boot-repair
boot-repair
# Click "Recommended repair"
```

### WiFi not working after install
- Connect via ethernet first
- Run: `sudo apt update && sudo apt install linux-firmware`

### Black screen after removing USB
- Hold power button 5 seconds to force off
- Power on → press F9 → select Linux Mint from boot menu

---

## Quick Reference — Keyboard Shortcuts in Linux Mint

| Shortcut | Action |
|----------|--------|
| Ctrl+Alt+T | Open Terminal |
| Super key | Open app menu |
| Alt+F4 | Close window |
| Ctrl+Alt+L | Lock screen |

---

*Guide prepared for: HP Victus, AMD Ryzen 5 5600H, 8GB RAM, Windows 11 dual boot*  
*Linux Mint 22.3 Cinnamon — Based on Ubuntu 24.04 LTS*
