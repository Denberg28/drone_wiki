# CFD Learning Path for UAV and Robotics Projects

A practical learning path for combining **Fusion 360**, **OpenFOAM**, **ParaView**, **Python**, and later **Nek5000/NekRS** for UAV, VTOL, propeller, rover, and thermal-design projects.

---

## Objective

Build a repeatable engineering workflow:

```text
Design Question
      ↓
Fusion 360 CAD
      ↓
Geometry Simplification
      ↓
Mesh and CFD Setup
      ↓
Simulation
      ↓
Post-Processing
      ↓
Physical Testing
      ↓
Design Revision
```

The goal is not only to produce CFD images. The goal is to create simulations that are:

- Reproducible
- Measurable
- Validated
- Useful for design decisions

---

## Recommended Toolchain

| Tool | Main Use |
| --- | --- |
| **Fusion 360** | Parametric CAD, assemblies, geometry cleanup, export |
| **OpenFOAM** | Main CFD solver for practical engineering studies |
| **ParaView** | Flow visualization and result analysis |
| **Python** | Data processing, plotting, automation, comparison |
| **WSL2 or Ubuntu** | Linux environment for OpenFOAM and scripting |
| **Nek5000 / NekRS** | Advanced high-order CFD and research studies |
| **Git / GitHub** | Version control and technical documentation |

---

# 1. Software Setup

## 1.1 Fusion 360

Use Fusion 360 as the geometry and design-management layer.

### Install

Download and install Fusion 360 from Autodesk.

During setup:

1. Create or sign in to an Autodesk account.
2. Select the appropriate personal, educational, or commercial license.
3. Confirm that the application can export:
   - STEP
   - STL
   - IGES
4. Enable cloud synchronization.
5. Create a dedicated CFD project folder.

### Recommended Project Structure

```text
Fusion 360 Project
├── Manufacturing Models
├── CFD Models
├── Components
├── Test Articles
└── Exported Geometry
```

### Essential Fusion 360 Skills

Learn these features first:

- User Parameters
- Components
- Sketch Constraints
- Construction Planes
- Loft
- Sweep
- Mirror
- Pattern
- Split Body
- Combine
- Surface Tools
- Interference Check
- Section Analysis
- Mass Properties
- STEP Export

### Suggested Parameters

```text
wing_span
root_chord
tip_chord
wing_sweep
wing_dihedral
motor_arm_length
tube_width
tube_height
propeller_diameter
propeller_clearance
battery_position
tail_arm_length
```

Keep the main design parameterized so dimensions can be changed without rebuilding the entire model.

---

## 1.2 Windows Subsystem for Linux

OpenFOAM is commonly used in Linux. On Windows, WSL2 provides a practical environment.

### Install WSL2

Open PowerShell as Administrator:

```powershell
wsl --install
```

Restart the computer when prompted.

Check the installation:

```powershell
wsl -l -v
```

Enter Ubuntu:

```powershell
wsl
```

Update packages:

```bash
sudo apt update
sudo apt upgrade -y
```

Install basic development tools:

```bash
sudo apt install -y \
    build-essential \
    git \
    curl \
    wget \
    cmake \
    python3 \
    python3-pip \
    python3-venv
```

---

## 1.3 OpenFOAM

Use OpenFOAM as the primary solver for practical project work.

### Installation Approach

Install the OpenFOAM release supported by your Ubuntu version.

After installation, load the OpenFOAM environment:

```bash
source /opt/openfoam*/etc/bashrc
```

Confirm the installation:

```bash
foamVersion
```

Create a working directory:

```bash
mkdir -p ~/OpenFOAM/$USER-run
cd ~/OpenFOAM/$USER-run
```

Copy a tutorial case:

```bash
cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .
cd pitzDaily
```

Run the case:

```bash
blockMesh
simpleFoam
```

Open the results:

```bash
paraFoam
```

### Core OpenFOAM Concepts

Every case normally contains:

```text
case/
├── 0/
│   ├── U
│   ├── p
│   └── turbulenceFields
├── constant/
│   ├── polyMesh/
│   ├── physicalProperties
│   └── turbulenceProperties
└── system/
    ├── controlDict
    ├── fvSchemes
    └── fvSolution
```

---

## 1.4 ParaView

ParaView is used to inspect CFD results.

### Install on Ubuntu

```bash
sudo apt install paraview -y
```

### Main Features to Learn

- Slice
- Clip
- Contour
- Stream Tracer
- Glyph
- Plot Over Line
- Calculator
- Integrate Variables
- Surface LIC
- Animation

### Minimum Result Review

For each case, inspect:

- Velocity field
- Pressure field
- Streamlines
- Separation regions
- Wake structure
- Wall shear
- Force coefficients
- Residual history

---

## 1.5 Python

Use Python to automate repetitive work and compare CFD with experiments.

### Create an Environment

```bash
python3 -m venv ~/cfd-env
source ~/cfd-env/bin/activate
```

Install common packages:

```bash
pip install numpy pandas matplotlib scipy jupyter
```

### Recommended Uses

- Plot lift and drag curves
- Compare mesh levels
- Process force coefficient files
- Calculate percentage error
- Organize test data
- Generate design-of-experiment tables
- Automate case creation

---

## 1.6 Git and GitHub

Use Git to track changes in:

- Geometry versions
- Solver settings
- Mesh settings
- Scripts
- Reports
- Experimental data

Initialize a repository:

```bash
git init
git add .
git commit -m "Initial CFD project structure"
```

Recommended `.gitignore`:

```gitignore
processor*/
postProcessing/
*.foam
*.vtk
*.vtu
*.log
constant/polyMesh/
```

Do not ignore documentation, scripts, setup files, or processed results.

---

## 1.7 Nek5000 and NekRS

Use Nek5000 or NekRS only after becoming comfortable with:

- Boundary conditions
- Meshing
- Turbulence models
- Convergence
- Validation
- Linux
- Python
- Numerical methods

Nek5000 is useful for:

- Spectral-element methods
- High-order CFD
- DNS
- LES
- Academic research
- Advanced turbulence studies

It should not be the first solver used for a complete VTOL aircraft.

---

# 2. CAD Preparation in Fusion 360

## 2.1 Maintain Two Models

```text
Manufacturing Model
├── Screws
├── Holes
├── Fillets
├── Wiring Openings
├── Mounts
└── Fabrication Details

CFD Model
├── Simplified Exterior
├── Closed Surfaces
├── No Threads
├── No Small Holes
├── No Wiring
└── Minimal Detail
```

Do not directly mesh the full manufacturing model unless all details are aerodynamically important.

---

## 2.2 Geometry Cleanup Checklist

Before export:

- Remove threads
- Remove logos
- Remove small fasteners
- Remove internal parts not exposed to flow
- Suppress tiny fillets
- Fill unnecessary holes
- Close gaps
- Remove overlapping bodies
- Confirm watertight geometry
- Check normal orientation
- Confirm dimensions and units

---

## 2.3 Export Format

Preferred order:

1. STEP for accurate CAD transfer
2. IGES when required
3. STL for triangulated workflows

Recommended filename:

```text
vtol-wing-v03-cfd.step
```

Avoid generic names such as:

```text
final.step
newfinal.step
final2.step
```

---

# 3. Learning Path

## Level 1 — CFD Fundamentals

### Focus

Learn the basic physics and simulation workflow.

### Topics

- Conservation of mass
- Momentum
- Energy
- Pressure
- Velocity
- Density
- Viscosity
- Reynolds number
- Boundary layers
- Flow separation
- Laminar and turbulent flow
- Numerical convergence
- Mesh independence

### Software Use

| Software | Activity |
| --- | --- |
| Fusion 360 | Create simple geometry |
| OpenFOAM | Run basic steady cases |
| ParaView | Inspect pressure and velocity |
| Python | Plot results |

### Exercises

1. Flow over a flat plate
2. Flow around a cylinder
3. Flow through a simple duct
4. Flow around a square tube
5. Flow around a basic airfoil

### Completion Criteria

Proceed when you can:

- Create a case
- Generate a mesh
- Assign boundary conditions
- Run a solver
- Check residuals
- Extract drag
- Explain the main flow features

---

## Level 2 — Geometry and Mesh Preparation

### Focus

Connect Fusion 360 to OpenFOAM.

### Workflow

```text
Fusion 360
      ↓
Simplified CFD Model
      ↓
STEP Export
      ↓
Surface Conversion
      ↓
Computational Domain
      ↓
Mesh Generation
      ↓
Mesh Inspection
```

### Skills

- CAD simplification
- Domain sizing
- Surface repair
- Background mesh
- Local refinement
- Boundary-layer inflation
- Mesh-quality checks

### OpenFOAM Tools

- `blockMesh`
- `snappyHexMesh`
- `surfaceFeatureExtract`
- `checkMesh`

### Minimum Mesh Checks

```bash
checkMesh
```

Review:

- Non-orthogonality
- Skewness
- Aspect ratio
- Negative volume
- Boundary definition
- Cell count

---

## Level 3 — External Aerodynamics

### Focus

Study drone and aircraft components before the complete vehicle.

### Recommended Projects

#### Motor Arm Drag

Compare:

- Square tube
- Square tube rotated 45 degrees
- Rounded square tube
- Streamlined fairing
- Airfoil-shaped fairing

Test at:

```text
5 m/s
10 m/s
15 m/s
```

Measure:

- Drag force
- Drag coefficient
- Wake width
- Separation position
- Added mass

#### Wing Study

Test:

- Angle of attack
- Wing taper
- Wing sweep
- Wingtip shape
- Control-surface deflection

Record:

```text
Lift coefficient
Drag coefficient
Lift-to-drag ratio
Pitching moment
Pressure distribution
Separation location
```

### Recommended Solver Progression

```text
Potential Flow Understanding
      ↓
Steady RANS
      ↓
k-omega SST
      ↓
Transient RANS
      ↓
Scale-Resolving Methods
```

---

## Level 4 — Near-Wall Treatment

### Focus

Understand boundary-layer meshing and `y+`.

### Two Common Strategies

#### Wall-Resolved

```text
Target y+ ≈ 1
Fine first cell
More layers
Higher computational cost
```

#### Wall Functions

```text
Target y+ within the selected wall-function range
Coarser near-wall mesh
Lower computational cost
```

### Record for Every Case

- Target `y+`
- Actual `y+`
- First-layer thickness
- Number of layers
- Growth rate
- Total inflation thickness

Do not use a near-wall mesh strategy without checking the turbulence model requirements.

---

## Level 5 — Propeller Modeling

### Stage 1 — Actuator Disk

Use for:

- Early aircraft studies
- Propeller wake approximation
- Wing interaction
- Motor placement comparison

Advantages:

- Fast
- Simple
- Low computational cost

### Stage 2 — Multiple Reference Frame

Use for:

- Approximate steady rotating flow
- Propeller pressure field
- Blade loading trends

### Stage 3 — Sliding Mesh

Use for:

- Time-dependent blade rotation
- Blade-passing effects
- Unsteady loads
- Detailed propeller-wing interaction

Begin with actuator-disk modeling. Do not begin with sliding mesh.

---

## Level 6 — VTOL Aerodynamics

Study separate operating conditions.

### Case 1 — Hover

```text
Lift motors active
Forward motor inactive
Near-zero freestream
```

Focus:

- Wing download
- Rotor wake
- Arm interference
- Ground effect

### Case 2 — Early Transition

```text
Lift motors active
Forward motor active
Low forward speed
```

Focus:

- Wake interaction
- Pitching moment
- Control authority
- Flow asymmetry

### Case 3 — Late Transition

```text
Reduced lift-motor contribution
Increasing forward speed
```

Focus:

- Wing lift recovery
- Rotor drag
- Stability
- Transition loads

### Case 4 — Cruise

```text
Forward motor active
Lift motors inactive
Normal forward airspeed
```

Focus:

- Aircraft drag
- Lift-to-drag ratio
- Motor-arm drag
- Propeller-wing interaction

---

## Level 7 — Thermal and Electronics Cooling

### Applications

- Flight controller enclosure
- ESC cooling
- Battery compartment
- Motor cooling
- Rover electronics
- Turret controller enclosure

### Fusion 360 Use

Model:

- Vent openings
- Internal channels
- Heat sinks
- Component position
- Air inlet and outlet

### CFD Outputs

- Air velocity
- Pressure drop
- Recirculation zones
- Low-flow regions
- Component temperature
- Heat rejection

Start with airflow only. Add thermal coupling after the flow model is stable.

---

## Level 8 — Correlation and Validation

Simulation must be compared with measurements.

```text
CFD
 ↕
Bench Test
 ↕
Wind Test
 ↕
Flight Test
```

### Motor and Propeller Data

Record:

- Thrust
- RPM
- Voltage
- Current
- Power
- Propeller size
- Battery state
- Air temperature

### Wing Data

Record:

- Airspeed
- Angle of attack
- Lift
- Drag
- Tuft behavior
- Stall behavior

### CFD Comparison

Calculate:

```text
Absolute Error = |Measured - Predicted|

Percentage Error =
|Measured - Predicted| / Measured × 100
```

A CFD result is not considered validated only because the contours appear realistic.

---

# 4. Project-Specific Learning Tracks

## 4.1 VTOL Track

1. Parametric airframe in Fusion 360
2. Motor-arm drag study
3. Clean wing CFD
4. Angle-of-attack sweep
5. Control-surface study
6. Forward-motor actuator disk
7. Lift-motor actuator disks
8. Hover wing-download study
9. Transition snapshots
10. Flight-test correlation

---

## 4.2 Propeller Track

1. Propeller terminology
2. Thrust-stand construction
3. RPM and electrical measurement
4. Simplified propeller CAD
5. Actuator-disk model
6. Multiple-reference-frame model
7. CFD and thrust-stand comparison
8. Diameter and pitch study
9. Efficiency mapping
10. Structural safety review

---

## 4.3 Rover and Turret Track

1. Parametric chassis
2. Center-of-gravity analysis
3. Wheel and motor sizing
4. Structural load simulation
5. Electronics cooling
6. Turret inertia
7. Vibration isolation
8. Sensor field of view
9. Environmental enclosure
10. Physical validation

---

# 5. Suggested First Project

## Aerodynamic Comparison of Bare and Faired Square Motor Arms

### Objective

Determine whether a lightweight fairing reduces the aerodynamic drag of the square carbon motor arms used in a small VTOL aircraft.

### Geometry

Create these variants in Fusion 360:

1. Bare square tube
2. Square tube rotated 45 degrees
3. Rounded leading edge
4. Symmetrical fairing
5. Airfoil-shaped fairing

### Conditions

```text
Velocity: 5, 10, and 15 m/s
Air: Standard atmospheric condition
Flow: External
Orientation: Multiple yaw angles
```

### Outputs

- Drag force
- Drag coefficient
- Wake width
- Pressure distribution
- Separation point
- Added fairing mass
- Drag reduction per gram

### Physical Test

Use:

- Small wind source
- Digital scale or load cell
- Printed or fabricated test pieces
- Fixed test distance
- Repeatable orientation

### Final Decision Metric

```text
Design Value =
Drag Reduction / Added Mass
```

---

# 6. Case Documentation Standard

Each CFD case should contain:

```text
Objective
Geometry version
CAD export version
Solver name and version
Flow condition
Fluid properties
Boundary conditions
Turbulence model
Mesh size
Near-wall treatment
Target y+
Actual y+
Convergence criteria
Force coefficients
Mesh-independence result
Physical-test comparison
Known limitations
Conclusion
```

---

# 7. Recommended Repository Structure

```text
project-name/
├── README.md
├── docs/
│   ├── requirements.md
│   ├── methodology.md
│   └── validation-plan.md
├── cad/
│   ├── manufacturing-model/
│   ├── cfd-model/
│   └── exports/
├── cfd/
│   ├── baseline/
│   ├── mesh-study/
│   ├── design-variants/
│   └── post-processing/
├── scripts/
│   ├── case-generator/
│   ├── data-processing/
│   └── plotting/
├── testing/
│   ├── bench/
│   ├── wind/
│   └── flight/
├── data/
│   ├── raw/
│   └── processed/
└── reports/
```

---

# 8. File Naming Standard

Use descriptive names:

```text
vtol-wing-v03-manufacturing.f3d
vtol-wing-v03-cfd.step
vtol-wing-v03-a05-10ms
vtol-wing-v03-mesh-coarse
vtol-wing-v03-mesh-medium
vtol-wing-v03-mesh-fine
```

Recommended pattern:

```text
project-component-version-condition
```

---

# 9. Mathematics and Programming Path

## Mathematics

Learn progressively:

1. Vectors
2. Matrices
3. Linear systems
4. Derivatives
5. Partial derivatives
6. Integrals
7. Differential equations
8. Coordinate transformations
9. Dimensional analysis
10. Numerical methods

## Programming

Recommended sequence:

```text
Python
  ↓
Linux Shell
  ↓
OpenFOAM Dictionaries
  ↓
C++ Fundamentals
  ↓
OpenFOAM Source Code
  ↓
Advanced Solver Development
```

Python and Linux automation should come before modifying solver source code.

---

# 10. Professional Focus

A practical long-term specialization for these projects is:

> UAV aerodynamic design, CFD methodology, CAD-to-simulation automation, and experimental correlation.

This combines:

- Fusion 360
- OpenFOAM
- ParaView
- Python
- Drone hardware
- Propulsion testing
- Flight testing
- Research documentation
- Student instruction

---

## Core Principle

```text
A simulation is useful only when its assumptions,
limitations, and physical correlation are understood.
```
