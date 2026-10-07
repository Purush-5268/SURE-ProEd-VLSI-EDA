# 🎓 Student Workflow: 10 Physical Design Projects

Welcome to the Physical Design Projects module! In this module, you will execute 10 medium-to-advanced Physical Design projects—from synthesis all the way to signoff—using industry-standard open-source tools. 

To successfully complete these 10 projects, maintain repository organization, and avoid critical storage issues in your cloud environment, **you must strictly follow the roadmap below.**

---

## 1. Clone Your Personal Repo inside the Master Workspace (Day 1 Setup)

Open your running Codespace, and inside the VS Code terminal, navigate to the workspace and clone your personal repository to store all project deliverables:

```bash
cd /workspaces/SURE-ProEd-VLSI-EDA
# Replace the URL with your actual assigned SURE Trust repository URL
git clone https://github.com/sure-trust/YourName-vlsi.git
cd YourName-vlsi
```

*Verification:* Type `pwd` to ensure you are inside your personal repository directory before writing any project code.

---

## 2. Organize Project Folders (Per Project)

Create a clean, standardized folder structure for each project so your trainer can easily review your synthesis reports, timing constraints, and final GDS outputs:

```text
YourName-vlsi/
├── project_1_8bit_alu/
│   ├── rtl/               # Verilog source files
│   ├── constraints/       # SDC files
│   ├── reports/           # Synthesis, STA, DRC, and LVS reports
│   └── scripts/           # OpenROAD / Yosys Tcl run scripts
├── project_2_pipelined_alu/
└── ...
```

---

## 3. Run the Synthesis-to-Signoff Flow (Execution Flow)

For every project, execute the manual or automated flow, gathering your mandatory deliverables (area reports, skew/latency calculations, and final GDS outputs). 

- Use the **VS Code terminal** for executing scripts and OpenROAD.
- Open the **VNC desktop** (Port 6080) only when inspecting physical layouts in **Magic** or viewing waveforms in **GTKWave**.

---

## 4. Commit Code and Clear Temporary Runs (Storage Management)

At the conclusion of each project, commit only your essential configuration files, scripts, and reports to GitHub, then wipe the heavy build directories. **This is critical to prevent your Codespace from running out of disk space during the 10 projects.**

```bash
git add .
git commit -m "Completed Project 1: 8-bit ALU Physical Design Signoff"
git push origin main

# Free up disk space for the next project
rm -rf runs/
```
