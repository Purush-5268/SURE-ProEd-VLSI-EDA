# 💻 Local Installation Guide: SURE ProEd VLSI Tools

If you prefer **not** to use the GitHub Codespace (cloud environment) and want to utilize your laptop's full RAM and CPU, you can install the complete open-source EDA toolchain locally. 

This guide replicates the exact environment we use in the cloud. It is designed for **Ubuntu 22.04** (either running natively via Dual-Boot or via Windows Subsystem for Linux - WSL2).

---

## Prerequisites
- **OS:** Ubuntu 22.04 LTS or Any LTS Your wish (Native or WSL2 on Windows 10/11)
- **RAM:** Minimum 8GB (16GB+ recommended for large SoC routing)
- **Disk Space:** At least 30GB of free space

---

## Step 1: System Update & Core Dependencies
First, open your Ubuntu terminal and update your system packages. We will install the basic VLSI tools (Yosys, Icarus Verilog, GTKWave, and Magic) along with compilers.

```bash
sudo apt-get update && sudo apt-get upgrade -y

sudo apt-get install -y \
    build-essential gcc g++ make pkg-config \
    python3 python3-pip python3-venv git wget curl ca-certificates \
    iverilog gtkwave yosys magic klayout netgen \
    gedit vim dbus-x11
```

---

## Step 2: Install OpenROAD
OpenROAD is the core engine for physical design (floorplanning, placement, CTS, routing). We will install the pre-compiled Ubuntu 22.04 binary.

```bash
cd /tmp
wget -q https://github.com/Precision-Innovations/OpenROAD/releases/download/2023-08-10/openroad_2.0_amd64-ubuntu22.04-2023-08-10.deb

# Install the downloaded package
sudo apt-get install -y ./openroad_2.0_amd64-ubuntu22.04-2023-08-10.deb

# Clean up
rm -f openroad*.deb
```

---

## Step 3: Install Volare & OpenLane
We use `pip3` to install **OpenLane** (the automated flow wrapper) and **Volare** (the open-source PDK manager).

*(Note: On newer Ubuntu versions, if you see an `externally-managed-environment` error due to PEP 668, append `--break-system-packages` to the command below or use a Python virtual environment).*

```bash
sudo pip3 install volare openlane --break-system-packages
```

---

## Step 4: Install the SkyWater 130nm PDK (SKY130)
The PDK (Process Design Kit) contains all the standard cells and physical rules necessary to turn your Verilog into real silicon layout. Because you are installing locally and have sufficient storage, we will download the **complete** PDK (including High Density, High Speed, Low Power, and all other variants). We use Volare to download the pre-compiled files.

```bash
# Create a directory for the PDK and take ownership
sudo mkdir -p /opt/pdk
sudo chown -R $USER:$USER /opt/pdk

# Use Volare to download the ENTIRE SKY130 PDK (approx. 15GB - this may take a few minutes)
volare enable --pdk sky130 --pdk-root /opt/pdk --include-libraries all bdc9412b3e468c102d01b7cf6337be06ec6e9c9a
```

---

## Step 5: Configure Environment Variables
Your tools need to know where the PDK is installed. Add the `PDK_ROOT` variable to your bash profile so it loads every time you open a terminal.

```bash
echo 'export PDK_ROOT=/opt/pdk' >> ~/.bashrc
echo 'export PDK=sky130A' >> ~/.bashrc

# Reload your bash profile
source ~/.bashrc
```

---

## 🎉 Verification Checklist
Once you finish running the installation, execute this quick check in your terminal:

```bash
yosys -V && openroad -version && magic --version && openlane --version
```

If all commands return version numbers without throwing missing library errors, **your local machine is 100% ready to tackle all 10 medium-to-advanced Physical Design industry projects from synthesis to signoff!**

You can now clone your personal SURE Trust repository and follow the exact same project workflows listed in the `STUDENT_PROJECT_WORKFLOW.md` directly on your own machine!
