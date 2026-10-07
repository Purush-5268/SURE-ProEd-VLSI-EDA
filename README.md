# 🚀 SURE ProEd Cloud VLSI Environment

Welcome to the official cloud workspace for the SURE ProEd VLSI training program! This repository provisions a complete, open-source RTL-to-GDS toolchain operating entirely in your web browser. 

You do **not** need to install anything on your local laptop. The cloud machine provides OpenLane, Yosys, Magic, OpenROAD, and the SkyWater 130nm PDK.

---

## 💻 1. Launching Your Cloud Workspace
1. Click the green **`<> Code`** button at the top right of this repository.
2. Select the **Codespaces** tab.
3. Click the **Create codespace on main** button.
4. *Note: The very first time you do this, it may take 5–10 minutes to download the PDK and build the environment. Please be patient.*

---

## 🖥️ 2. Accessing the Graphical Desktop (GUI)
Because VLSI involves physical layouts, you need a desktop interface.
1. Once your VS Code environment loads in the browser, look at the bottom terminal panel.
2. Click the **Ports** tab.
3. Find **Port 6080 (noVNC Desktop)**.
4. Click the **Globe Icon** (Forwarded Address) next to it.
5. A new browser tab will open displaying a full Linux XFCE desktop. 
6. Right-click anywhere on the Linux desktop and select **Open Terminal Here** to begin working.

---

## 📂 3. Cloning Your Individual Project Repository
**CRITICAL:** You only have "Read" access to this master environment. To save your actual assignment files, you must clone your personal SURE ProEd repository inside the cloud machine.

In your Codespace terminal (or XFCE terminal), run:
```bash
# Clone your assigned repository (replace with your actual GitHub username/repo)
git clone [https://github.com/SURE-ProEd-Org/YourName-VLSI-Projects.git]

# Move into your project workspace
cd YourName-VLSI-Projects

```

*All your RTL scripts, STA reports, and GDS layouts MUST be saved inside this folder.*

---

## 🛠️ 4. VLSI Tool Command Cheat Sheet

All tools are pre-installed and added to your system path.

### 🔹 Logic Synthesis (Yosys)

```bash
yosys
yosys> read_verilog your_design.v
yosys> synth -top your_design
yosys> show

```

### 🔹 Digital Simulation (Icarus Verilog & GTKWave)

```bash
iverilog -o sim_out your_design.v tb_your_design.v
vvp sim_out
gtkwave dump.vcd &

```

### 🔹 Automated RTL-to-GDS Flow (OpenLane)

```bash
# Start the OpenLane Docker flow
make mount
# Inside the OpenLane prompt:
./flow.tcl -design <your_design_folder> -init_design_config
./flow.tcl -design <your_design_folder>

```

### 🔹 Physical Layout & DRC (Magic)

```bash
# Open a GDS file with the Sky130 technology file
magic -T sky130A your_layout.gds &

```

### 🔹 Static Timing Analysis (OpenSTA)

```bash
sta your_sta_script.tcl

```

---

## 💾 5. Saving Your Work (Before Closing!)

⚠️ **WARNING:** Codespaces are temporary. If you do not push your work to your personal repository, **IT WILL BE DELETED** by GitHub after a period of inactivity.

At the end of every session, run these commands inside your project folder:

```bash
git add .
git commit -m "Completed synthesis for Project 4"
git push origin main

```

---

## ⚠️ Important Limitations

* **RAM Limits:** This free cloud environment is capped at **8 GB of RAM**. It is perfect for block-level design, IPs, and standard cells.
* **Massive SoC Routing:** If you are running the final full-chip RISC-V SoC routing, the tool will likely crash (Out of Memory). For the massive Capstone project, you must set up the tools locally via WSL2 to utilize your laptop's full RAM.
* **Timeouts:** Do not close your browser tab while a long routing job is running. The cloud machine will go to sleep after 30 minutes of inactivity and kill your job. Keep the tab open and active!

```
