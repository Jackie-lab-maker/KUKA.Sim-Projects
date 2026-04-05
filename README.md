# Digital Twin for Composite Filament Winding 

[](https://www.google.com/search?q=https://www.opmobility.com/)
[](https://www.google.com/search?q=https://www.kuka.com/)
[](https://www.google.com/search?q=%23)


## 📌 Project Overview

The primary objective was to create a "Single Source of Truth" between the virtual KUKA simulation and the physical winding cell. This allowed for rapid iteration on path planning and sensor validation without wasting expensive composite materials.

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

### 2\. Dome & Transition Consistency

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

## 💻 Code Snippet: Path Velocity Scaling

The following KRL logic (pseudo-code) demonstrates how winding speed is adjusted during the transition to the dome to maintain tension consistency.

```krl
;&ACCESS RVP
;&REL 1
DEF Winding_Transition( )
  ; Initialize Winding Parameters
  BAS(#PTP_PARAMS, 100)
  $VEL.CP = 0.5 ; Standard hoop speed (m/s)
  
  ; Move to Hoop-to-Dome Transition Point
  PTP P_Hoop_End
  
  ; Slow down for the dome to ensure encoder accuracy
  ; and prevent fiber slippage
  $VEL.CP = 0.15 
  LIN P_Dome_Start C_DIS
  
  ; Execute Geodesic Winding Path
  FOR i = 1 TO 50
    LIN Dome_Path[i] C_DIS
    ; Logic to sync Rotary Encoder feedback
    IF $IN[1] == TRUE THEN
       ; Trigger Gage R&R Data Capture
       Log_Encoder_Position()
    ENDIF
  ENDFOR
END
```

-----

## 📈 Results & Impact

| Metric | Before Digital Twin | After Digital Twin |
| :--- | :--- | :--- |
| **Angle Deviation** | $\pm 2.5^{\circ}$ | $\pm 0.4^{\circ}$ |
| **Trial Material Waste** | High (Physical Prototyping) | Near Zero (Virtual Validation) |
| **Encoder Reliability** | Unverified | Gage R\&R Validated |

-----

## ⚖️ License & Confidentiality

**Note:** All data, CAD models, and images associated with this project are the intellectual property of **OPmobility**. This repository contains sanitized code and documentation approved for public demonstration of technical competency.

*For inquiries regarding the methodology, please contact the repository owner.*m
