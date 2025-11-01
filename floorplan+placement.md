# Floorplan and Placement of GCD 

This laboratory guide provides comprehensive instructions for implementing **Floorplanning** and **Placement** stages of the **Greatest Common Divisor (GCD)** circuit using **OpenROAD** Flow Scripts. The workflow demonstrates the critical transition from logical design representation to physical layout implementation in the VLSI backend design flow.

---

Table of Contents

1. [Overview](#overview)
2. [Prerequisites and Environment Setup](#prerequisites-and-environment-setup)
3. [Importance of Floorplanning and Placement](#importance-of-floorplanning-and-placement)
   - [Installation of OpenROAD](#installation-of-openroad)
   - [Installation of Flow Scripts](#installation-of-flow-scripts)
4. [Floorplan of gcd](#floorplan-of-gcd)
5. [Placement of gcd](#placement-of-gcd)
6. [Summary](#summary)

---

## Overview

This tutorial demonstrates the physical design implementation of a GCD circuit, focusing on two critical stages of the VLSI backend flow:

- **Floorplanning**: Defining the chip's physical boundaries, I/O pad placement, and macro positioning
- **Placement**: Positioning standard cells within the defined floorplan area

These stages establish the foundation for subsequent routing and timing closure operations.



## Prerequisites and Environment Setup

### System Requirements

Ensure your development environment meets the following specifications:

- **Operating System**: Ubuntu 20.04 or later (Ubuntu 22.04 recommended)

- **Version Control**: Git (latest stable version)
- **Build Toolchain**:

   - CMake (version 3.14 or higher)
   - GCC/G++ compiler suite
   - GNU Make
   - Standard development libraries

- **Python Environment**: Python 3.6 or higher

- **Required EDA Tools**
The following Electronic Design Automation tools must be available:

- **OpenROAD**: Physical design automation platform
- **Yosys**: Logic synthesis framework
- **OpenSTA**: Static timing analysis engine

---

## Importance of Floorplanning and Placement

Floorplanning is a crucial stage in the physical design process that defines the overall structure and layout of the chip prior to cell placement. It includes:

- Determining the die and core dimensions.
- Positioning I/O pads along the die periphery.
- Strategically arranging macros (such as SRAMs and PLLs).
- Organizing the core area into standard cell rows.
- Designing an efficient power distribution network (PDN).
- Reserving regions for physical-only cells (e.g., tap cells, endcaps, and blockages).

Placement follows floorplanning and involves arranging standard cells and smaller functional blocks within the defined core area. The main objectives are to:

- Minimize interconnect wire length between connected cells.
- Reduce routing congestion to facilitate efficient interconnections.
- Satisfy timing requirements to ensure reliable circuit performance.

---

### Installation of OpenROAD

Firstly, `or-tools` needs to be installed,

```bash
git clone https://github.com/google/or-tools.git
cd or-tools
```

then, configure the build

```bash
cmake -S . -B build -DBUILD_DEPS=ON
```

and install the `or-tools` using,

```bash
cmake --build build --config Release --target install -v
```

---

Then to set OpenROAD up,

- Clone the official **OpenROAD repository**.

```bash
git clone --recursive git clone https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD
```

- Initialize and update its submodules to include all dependencies.

```bash
git submodule update --init --recursive
```

- Compile the source code to generate the OpenROAD binaries.

```bash
rm -rf build
mkdir build
cd build
sudo cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-Wno-error" -DCMAKE_PREFIX_PATH="/usr/local" -DCMAKE_CXX_COMPILER=/usr/bin/g++-9
```

---

### Installation of Flow Scripts

Clone the [OpenROAD Flow Scripts repo](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts) inside a folder of your choice

```bash 
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```

Then to map the installed OpenROAD, and previously installed OpenSTA and Yosys, simply following the commands below,

```bash
export OPENROAD_EXE=/usr/bin/openroad
export YOSYS_EXE=/usr/bin/yosys
export OPENSTA_EXE=/usr/bin/sta
```
Use `whereis` command to locate the binaries of OpenROAD, Yosys and OpenSTA


With this OpenROAD and its script flow installation is complete.

---

### After installing Lemon via:

```bash
sudo apt-get update
sudo apt-get install -y liblemon-dev
lemon --version
```
If that prints LEMON, `version 1.3.1` (or similar), everything’s fine.

If it isn't working, then update all packages in ubuntu and you can able to see this by using the following command 
```bash
sudo apt list --upgradable
```

---


### Check whether Docker is installed

```bash
docker --version
```
If Docker is installed, you’ll see something like:
```bash
Docker version 27.1.1, build ...
```
If you get:
```
Command 'docker' not found
```
then it’s not installed yet.
then

### Run the official install commands:
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Check that Docker daemon is running

After installation:
```bash
sudo systemctl status docker
```

If it says inactive (dead), start it:
```bash
sudo systemctl start docker
```

and enable auto-start:
```bash
sudo systemctl enable docker
```
Next step: run the OpenROAD Flow Docker container

The image name in your earlier command was outdated.
Use the official current image instead:
```bash
sudo docker run --rm -it \
  -v $(pwd):/OpenROAD-flow-scripts \
  openroad/flow:latest
```
Explanation:

- --rm → automatically cleans up the container when it exits

- -it → interactive terminal

- -v $(pwd):/OpenROAD-flow-scripts → mounts your current folder into the container

- openroad/flow:latest → official OpenROAD Docker image (replaces the old “flow-ubuntu22.04-builder” image)


```bash
export OPENROAD_EXE=/usr/bin/openroad
export YOSYS_EXE=/usr/bin/yosys
export OPENSTA_EXE=/usr/bin/sta
cd ~/OpenROAD-flow-scripts
./setup.sh -tool yosys
cd ~/OpenROAD-flow-scripts/flow
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk
```




---

## Summary

This laboratory successfully demonstrates the implementation of Floorplanning and Placement stages for the GCD module using OpenROAD Flow Scripts, effectively bridging the gap between logic-level design and physical layout in the VLSI backend design flow.

### Workflow Overview
The implementation followed a systematic three-phase approach:

**Phase 1**: Development Environment Configuration

- Successfully installed and integrated essential EDA tools including OpenROAD, Yosys, OpenSTA, and OR-Tools
- Configured a robust development environment on Ubuntu-based Linux distribution (Ubuntu/Zorin OS) with necessary dependencies: Git, CMake, GCC compiler suite, and Python 3.6+
- Compiled OpenROAD from source code and established proper build configuration
- Deployed OpenROAD Flow Scripts with correct binary linkages to installed tool executables

**Phase 2**: Floorplan Design Execution

- Accessed the workflow execution directory within OpenROAD-flow-scripts/flow
- Successfully generated the floorplan for the GCD design using automated make targets
- Conducted visual verification through the integrated GUI, confirming proper die boundaries, core area definition, and I/O pin placement

**Phase 3**: Physical Placement Implementation

- Executed the placement stage, optimizing standard cell positions within the floorplan
- Performed graphical inspection of placement results via GUI interface
- Validated the correct positioning of Sky130 standard cell library components with no design rule violations

**Key Achievements**
Upon completion of this laboratory exercise, the GCD module has been successfully transitioned through critical physical design stages:

- **Established Physical Foundation**: Created a well-defined floorplan with optimized die area and power distribution planning
- **Achieved Legal Placement**: Positioned all standard cells within design constraints, ensuring manufacturability
- **Prepared for Backend Flow**: Generated placement data ready for subsequent stages including clock tree synthesis, detailed routing, and comprehensive timing analysis

This work provides the essential groundwork for completing the full VLSI backend implementation flow, enabling progression toward tape-out readiness.

---
