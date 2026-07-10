# CFD Learning Tools

A curated collection of open-source tools and references for learning computational fluid dynamics.

---

## OpenFOAM

**OpenFOAM** is a free and open-source CFD software package developed primarily by OpenCFD Ltd.

It provides solvers and utilities for engineering and scientific applications involving:

* Laminar and turbulent flow
* Heat transfer
* Chemical reactions
* Multiphase flow
* Acoustics
* Solid mechanics
* Electromagnetics
* Compressible and incompressible flow

OpenFOAM includes extensive documentation, tutorials, meshing utilities, numerical solvers, and post-processing workflows.

**Recommended for:** beginners and advanced users studying general-purpose CFD, engineering simulation, solver development, and industrial applications.

[Open OpenFOAM Website](https://www.openfoam.com/)

---

## Nek5000

**Nek5000** is a highly scalable, open-source CFD solver based on the spectral element method.

It is designed for high-fidelity simulations, including:

* Direct numerical simulation
* Large-eddy simulation
* Incompressible fluid flow
* Heat transfer
* Magnetohydrodynamics
* Unsteady flow analysis

The documentation includes installation guidance, case setup, meshing tools, solver theory, tutorials, visualization, and post-processing resources.

**Recommended for:** advanced CFD research, high-performance computing, DNS, LES, and spectral-element-method studies.

[Open Nek5000 Documentation](https://nek5000.github.io/NekDoc/)

---

## Suggested Learning Path

```text
1. Fluid mechanics fundamentals
        ↓
2. Numerical methods
        ↓
3. Mesh generation
        ↓
4. Boundary and initial conditions
        ↓
5. OpenFOAM introductory tutorials
        ↓
6. Solver verification and validation
        ↓
7. Turbulence modelling
        ↓
8. Nek5000 spectral-element simulations
        ↓
9. DNS and LES applications
```

---

## Basic CFD Workflow

```text
Define the physical problem
        ↓
Create the computational domain
        ↓
Generate the mesh
        ↓
Select the governing equations
        ↓
Set material properties
        ↓
Apply boundary conditions
        ↓
Configure the numerical solver
        ↓
Run the simulation
        ↓
Check convergence
        ↓
Validate and interpret the results
```

---

## Recommended Starting Point

| Experience Level | Recommended Tool     | Focus                                       |
| ---------------- | -------------------- | ------------------------------------------- |
| Beginner         | OpenFOAM             | General CFD workflow and tutorials          |
| Intermediate     | OpenFOAM             | Turbulence, heat transfer, and custom cases |
| Advanced         | Nek5000              | High-order methods, DNS, LES, and HPC       |
| Research         | OpenFOAM and Nek5000 | Solver comparison and validation            |

---

## Important Notes

> [!NOTE]
> CFD results should be verified through mesh-independence studies, convergence monitoring, analytical solutions, experimental data, or published benchmarks.

> [!WARNING]
> A completed simulation does not automatically represent a physically correct solution. Incorrect mesh quality, boundary conditions, turbulence models, or numerical settings can produce misleading results.

---

## References

* [Nek5000 Documentation](https://nek5000.github.io/NekDoc/)
* [OpenFOAM](https://www.openfoam.com/)
