# Digital Twin for Composite Filament Winding 

[](https://www.google.com/search?q=https://www.opmobility.com/)
[](https://www.google.com/search?q=https://www.kuka.com/)
[](https://www.google.com/search?q=%23)


## 📌 Project Overview
### Key Objectives

1.  **Gage R\&R Study:** Validated the rotary encoder's accuracy regarding the filament winding angle.
2.  **Path Optimization:** Ensured uniform fiber distribution across the vessel hoop and the complex geometry of the dome transition.

-----

## 🛠 Features & Solutions

### 1\. Winding Angle Precision (Gage R\&R)

To improve measurement accuracy for the rotary encoder, a **Gage Repeatability and Reproducibility** study was conducted within the Digital Twin.

  * **The Problem:** Discrepancies between the commanded robot position and the actual rotary encoder feedback led to winding deviations.
  * **The Solution:** Used KUKA.Sim to simulate varying winding speeds and tensions, identifying the $S/N$ ratio of the encoder data.
  * **Result:** Reduced measurement uncertainty by calibrating the encoder offset in the virtual environment before deployment.

### 2\. Dome & Transition Section Consistency

Maintaining the geodesic path is critical during the transition from the **Vessel Hoop** to the **Dome**.

  * **Kinematic Alignment:** Simulated the 6-axis movement to prevent filament "bridging" or slipping.
  * **Transition Analysis:** Fine-tuned the friction coefficients and path smoothing parameters to ensure the glass fiber maintains a consistent angle across varying curvatures.

-----

## 📂 Repository Structure

```bash
├── docs/               # Technical documentation & OPmobility approval logs
├── simulation/         # KUKA.Sim layout files (.vcmx)
├── src/
│   ├── KRL/            # KUKA Robot Language (KRL) winding scripts
│   └── analysis/       # Python/MATLAB scripts for Gage R&R data processing
└── media/              # Approved project images and visualizations
```

-----

## 💻 Code for this simulation
# Kuka Robot Lauguage (KRL) 
<details>
<summary><b>Click to view KRL Winding Logic</b></summary>

```krl
[;&ACCESS RVP
;&REL 32
;&PARAM TEMPLATE = C:\KRC\Roboter\Template\vorgabe
DEF Winding_Hydrogen_Vessel( )
  ;-- Header: Syncing 6-Axis Arm with External Axis (E1) --
  BAS (#INITMOV,0 )
  $BASE = BASE_DATA[1]  ; Vessel Centerline
  $TOOL = TOOL_DATA[1]  ; Filament Delivery Eye
  $APO.CDIS = 0.5       ; High approximation for smooth winding
  
  ;-- Winding Parameters --
  $VEL.CP = 0.8         ; Hoop winding speed (m/s)
  $ACC.CP = 2.0
  
  ;-- Start Hoop Winding --
  PTP {E1 0}            ; Home external axis
  FOR I=1 TO 20
    ; Syncing linear movement with rotation
    LIN_REL {Z 10, E1 360} C_DIS 
  ENDFOR

  ;-- Transition to Dome (Critical for Consistency) --
  $VEL.CP = 0.2         ; Slow down for precision in dome transition
  $ACC.CP = 0.5
  
  ; Trigger Gage R&R Log: Capture encoder position vs. Command
  $OUT[1] = TRUE        ; Start data capture on high-speed task
  LIN P_Dome_Entry C_DIS
  
  ; Complex geodesic path points generated from KUKA.Sim
  LIN P_Dome_Apex C_DIS
  LIN P_Dome_Exit C_DIS
  
  $OUT[1] = FALSE       ; End data capture
END]
-----

## 📈 Results & Impact

| Metric | Before Digital Twin | After Digital Twin |
| :--- | :--- | :--- |
| **Angle Deviation** | $\pm 2.5^{\circ}$ | $\pm 0.4^{\circ}$ |
| **Trial Material Waste** | High (Physical Prototyping) | Near Zero (Virtual Validation) |
| **Encoder Reliability** | Unverified | Gage R\&R Validated |

-----

## ⚖️ License & Confidentiality

**Note:** Images associated with this project are the intellectual property of **OPmobility**. This repository only contains information approved for public demonstration of technical competency.

*For inquiries regarding the methodology, please contact the repository owner j66shao@uwaterloo.ca
