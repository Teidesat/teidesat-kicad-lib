# TeideSat KiCad Master Library 🚀

Welcome to the official KiCad library repository for **TeideSat**. This repository contains all the shared schematic symbols, footprints, and 3D models used by our hardware engineering team. 

By strictly following these guidelines, we ensure that our BOMs are automatically generated, our 3D renders are flawless, and our PCB manufacturing process is error-free.

---

## 1. Installation & Setup 🛠️

To use this library, you must configure your local KiCad installation to point to this repository using an Environment Variable. This ensures the library works seamlessly across Windows, Mac, and Linux.

### Step 1: Clone the Repository
Clone this repository to a secure location on your hard drive:
```bash
git clone https://github.com/teidesat/teidesat-kicad-lib.git
```

### Step 2: Set the Environment Variable
1. Open KiCad (Main Project Window).
2. Go to **Preferences > Configure Paths**.
3. Add a new variable named exactly: **`TEIDESAT_LIB`**.
4. Set the **Path** to the local directory where you cloned this repository (e.g., `C:/Projects/teidesat-kicad-lib` or `/home/user/teidesat-kicad-lib`).
5. Click **OK**.

### Step 3: Add the Libraries to KiCad
1. Go to **Preferences > Manage Symbol Libraries**.
2. Under the *Global Libraries* tab, click the folder icon to add existing libraries.
3. Select all the `.kicad_sym` files inside the `/symbols/` folder of this repo.
4. Go to **Preferences > Manage Footprint Libraries**.
5. Add all the `.pretty` folders inside the `/footprints/` folder.

*(Note: Once added, KiCad will store the paths using the `${TEIDESAT_LIB}` variable automatically).*

---

## 2. Library Structure 📁

Do not create single-component libraries. All new components must be categorized into the following structure:

### Schematic Symbols (`/symbols/*.kicad_sym`)
Organized by **function**:
* `TEIDESAT_Passives`: Resistors, capacitors, inductors, ferrite beads.
* `TEIDESAT_Discretes`: Diodes (Rectifier, Zener, LEDs) and Transistors.
* `TEIDESAT_IC_Power`: LDOs, Buck/Boost converters, voltage references.
* `TEIDESAT_IC_MCU`: Microcontrollers and microprocessors.
* `TEIDESAT_IC_Digital`: Memory, logic gates, level shifters.
* `TEIDESAT_IC_Analog`: Op-Amps, comparators, ADCs, DACs.
* `TEIDESAT_IC_Sensors`: Accelerometers, gyroscopes, temperature, IMUs.
* `TEIDESAT_IC_Transceivers`: RS-485, CAN, RF modules, LoRa.
* `TEIDESAT_Connectors`: Pin headers, USB, terminal blocks.
* `TEIDESAT_Electromechanical`: Relays, pushbuttons, switches.
* `TEIDESAT_Mechanical`: Mounting holes, fiducials, test points.

### Footprints (`/footprints/*.pretty`)
Organized by **physical package/shape**:
* `TEIDESAT_Passives_SMD.pretty`: 0402, 0603, 0805, 1206.
* `TEIDESAT_Package_SOT.pretty`: SOT-23, SOT-223, SOT-89.
* `TEIDESAT_Package_SOIC.pretty`: SOIC, SSOP, TSSOP.
* `TEIDESAT_Package_QFN_QFP.pretty`: QFN, TQFP, LQFP.
* `TEIDESAT_Package_BGA.pretty`: Ball Grid Array packages.
* `TEIDESAT_Modules.pretty`: Pre-assembled boards (e.g., GPS modules, radio modules).
* `TEIDESAT_Connectors.pretty`: Headers, USB, RF connectors (SMA, U.FL).
* `TEIDESAT_Crystals_Oscillators.pretty`: Quartz crystals, MEMS.

### 3D Models (`/3dmodels/`)
The folder structure here **must strictly mirror** the `.pretty` folders.
Example: A footprint in `TEIDESAT_Package_SOIC.pretty` must have its `.step` file in `/3dmodels/TEIDESAT_Package_SOIC/`.

## 📂 Project Templates

To standardize the satellite's hardware and ensure all boards fit into the mechanical structure, we use **KiCad Templates**. These templates already include the form factor (PC/104), the official TeideSat Title Block, the layer stackup, and the Design Rule Checks (DRC) validated for manufacturing.

### Template Configuration (First-time setup only)
For KiCad to automatically detect our templates:

1. Open KiCad and go to **Preferences > Configure Paths...**.
2. Add a new environment variable by clicking the `+` button.
3. Name it exactly like this: `KICAD_USER_TEMPLATE_DIR`
4. For the path, select the `templates` folder located inside this repository (`path_to_the_repository/templates`).
5. Click **OK**.

### How to start a new board for the CubeSat
Never start a blank project! 

1. On the KiCad main screen, go to **File > New Project from Template...**.
2. You will see a new tab called **User Templates**.
3. Select the appropriate template for your subsystem (for example, `TeideSat_CubeSat_4L_Standard`).
4. Choose where to save your new project, and you are all set! You now have the Title Block, board outline, and design rules preconfigured.

---

## 3. Component Creation Rules ⚠️

### A. Atomic Components Only
Do not create generic symbols (like a generic "Resistor"). Every symbol must represent a real, purchasable component.
* **Good:** `RES_10K_1%_0603` (Has a specific footprint and MPN assigned).
* **Bad:** `R` or `Resistor_Generic`.

### B. Mandatory Custom Fields
Every schematic symbol **must** have the following properties filled out before being merged:
1. **Manufacturer:** e.g., *Texas Instruments*
2. **MPN (Manufacturer Part Number):** e.g., *STM32F103C8T6*
3. **Datasheet:** Valid URL or path to the PDF.

### C. 3D Model Linking
Never use absolute paths (like `C:/Users/...`) for 3D models.
Always use the environment variable in the footprint properties:
`${TEIDESAT_LIB}/3dmodels/TEIDESAT_Package_Name/Model_Name.step`

### D. Naming Conventions (Footprints)
Follow the KiCad Library Convention (KLC) / IPC-7351 where possible. Keep it readable but standardized.
* Example: `R_0603_1608Metric`

---

## 4. Git Workflow (How to Contribute) 🔄

To keep the library stable and error-free, follow the **Librarian Workflow**:
1. **Never push directly to `main`.**
2. When you need a new component, create a new branch: `git checkout -b feature/add-stm32-mcu`
3. Design the symbol, footprint, and add the 3D model.
4. Commit your changes and push the branch.
5. Open a **Pull Request (PR)**.
6. A designated Team Librarian will review the pinout, dimensions, and metadata. Once approved, it will be merged into `main`.
7. Run `git pull` frequently to ensure you have the latest components.

Happy designing! 🛰️