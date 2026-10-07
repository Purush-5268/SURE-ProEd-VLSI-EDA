# 🚀 SURE ProEd Cloud VLSI Environment

Welcome to the official cloud workspace for the SURE ProEd VLSI training program! This repository provisions a complete, open-source RTL-to-GDS toolchain operating entirely in your web browser. 

You do **not** need to install anything on your local laptop. The cloud machine provides OpenLane, Yosys, Magic, OpenROAD, and the SkyWater 130nm PDK.

**Why are you here?** 
You are not here just to click a "Run" button. You are here to become a **Real Physical Design (PD) Engineer**. Commercial EDA tools require deep knowledge of every step. This environment allows you to run **everything manually, step-by-step** (SDC constraints, floorplanning, decap cells, physical synthesis), or automate it with OpenLane.

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
**CRITICAL:** You only have "Read" access to this master environment. To save your assignments for all **10 projects**, you must clone your personal SURE Trust repository inside the cloud machine. **All 10 projects will be pushed to this single repository.**

In your Codespace terminal (or XFCE terminal), run:
```bash
# Clone your assigned repository (replace with your actual GitHub username/repo)
git clone https://github.com/sure-trust/YourName-vlsi.git

# Move into your project workspace
cd YourName-vlsi
```

*All your RTL scripts, STA reports, and Tcl scripts for all 10 projects MUST be saved inside this folder.*

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

### 🔹 Manual Physical Design Flow (OpenROAD)
For manual placement, SDC constraints, and routing (like a real PD engineer):
```bash
openroad
openroad> read_verilog synthesized_netlist.v
openroad> read_sdc constraints.sdc
openroad> initialize_floorplan -core_utilization 60 -aspect_ratio 1.0 -core_space 10
openroad> tapcell -distance 14
openroad> global_placement
openroad> detailed_route
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

## 💾 5. Saving Your Work & Storage Management

⚠️ **WARNING:** Codespaces are temporary. If you do not push your work to your personal repository, **IT WILL BE DELETED** by GitHub after a period of inactivity.

**CRITICAL STORAGE RULE:** 
You have 10 robust projects to complete. If you keep all the generated `.gds` files and massive `runs/` directories for all 10 projects, **your Codespace storage will crash and run out of space**. 

At the end of every project, you must:
1. Push your essential code (Verilog files, SDC constraints, TCL scripts, and timing reports) to your SURE Trust Github repo.
2. **DELETE** the massive generated output folders (like `runs/`) for that project inside the Codespace to free up storage before starting the next project!

```bash
git add .
git commit -m "Completed synthesis and routing for Project 1"
git push origin main

# AFTER PUSHING, delete the massive runs/ folder to save Codespace storage:
rm -rf runs/ 
```

---

## ⚠️ Important Limitations

* **RAM Limits:** This free cloud environment is capped at **8 GB of RAM**. It is perfect for block-level design, IPs, and standard cells.
* **Storage Limits:** Your Codespace has limited disk space (usually 32GB). You **must** delete old `runs/` and generated `.gds` files after pushing your scripts to GitHub, or the machine will run out of space during the 10 projects.
* **Massive SoC Routing:** If you are running the final full-chip RISC-V SoC routing, the tool will likely crash (Out of Memory). For the massive Capstone project, you must set up the tools locally via WSL2 to utilize your laptop's full RAM.
* **Timeouts:** Do not close your browser tab while a long routing job is running. The cloud machine will go to sleep after 30 minutes of inactivity and kill your job. Keep the tab open and active!
