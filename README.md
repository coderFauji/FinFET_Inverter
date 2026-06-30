# Characterization of a CMOS Inverter using ASAP7 7nm FinFET PDK

---

## 📌 Introduction

As CMOS technology continues to scale into the nanometer regime, conventional planar MOSFETs face increasing challenges due to short-channel effects and leakage currents. FinFET technology addresses these limitations by providing improved electrostatic control of the channel, making it the preferred device architecture for advanced technology nodes.

This project focuses on the design and characterization of a CMOS inverter using the **ASAP7 7nm FinFET Process Design Kit (PDK)**. The objective is to study the impact of transistor sizing by varying the number of fins in the PMOS and NMOS devices and evaluating key electrical characteristics through circuit simulation.

The inverter schematic was created using **Xschem**, while all simulations were performed using **ngspice**.

---

# 📖 Project Objectives

The primary objectives of this work are:

- Design a CMOS inverter using ASAP7 7nm FinFET devices.
- Perform DC and transient simulations.
- Obtain the Voltage Transfer Characteristics (VTC).
- Measure threshold voltage (Vth).
- Evaluate voltage gain.
- Calculate propagation delay.
- Estimate average power consumption.
- Analyze drain current characteristics.
- Compare the effect of different PMOS/NMOS fin configurations.

---

# 🛠 Software and Technology Used

| Software | Purpose |
|----------|---------|
| Xschem | Schematic Design |
| ngspice | Circuit Simulation |
| ASAP7 PDK | 7nm FinFET Device Models |
| Ubuntu Linux | Development Platform |

---

# 📂 Repository Organization

```
FinFET_Inverter
│
├── README.md
│
├── PMOS10_NMOS10
│   ├── inverter_vtc1.spice
│   ├── vtc.png
│   ├── delay.png
│   └── id.png
│
├── PMOS16_NMOS12
│   ├── inverter_vtc2.spice
│   ├── vtc.png
│   ├── delay.png
│   └── id.png
│
└── PMOS18_NMOS20
    ├── inverter_vtc3.spice
    ├── vtc.png
    ├── delay.png
    └── id.png
```

Each configuration folder contains the SPICE netlist along with the generated simulation plots corresponding to that transistor sizing.

---

# ⚙ Design Flow

The complete workflow followed during this project is illustrated below.

```
ASAP7 PDK
      │
      ▼
Create CMOS Inverter Schematic
      │
      ▼
Generate SPICE Netlist
      │
      ▼
Perform DC Analysis
      │
      ▼
Extract Voltage Transfer Characteristics
      │
      ▼
Perform Transient Analysis
      │
      ▼
Measure Delay, Power and Frequency
      │
      ▼
Compare Different Fin Configurations
```

---

# 📐 Device Configurations

Three inverter configurations were characterized.

| Configuration | PMOS nfin | NMOS nfin |
|--------------|----------:|----------:|
| Configuration 1 | 10 | 10 |
| Configuration 2 | 16 | 12 |
| Configuration 3 | 18 | 20 |

---

# 📊 Characterization Results

| PMOS | NMOS | Vth (V) | Id (A) | Power (W) | Delay (ps) | Gain | Frequency (GHz) |
|------:|------:|---------:|---------:|---------:|----------:|------:|--------------:|
| 10 | 10 | 0.345064 | 5.8599E-07 | 2.1317E-05 | 25.297 | 6.424 | 22.467 |
| 16 | 12 | 0.337759 | 1.1718E-06 | 4.0497E-05 | 25.221 | 6.432 | 22.516 |
| 18 | 20 | 0.364614 | 7.0231E-07 | 2.9181E-05 | 25.466 | 6.444 | 22.289 |

---

# 📈 Simulation Results

## Configuration 1 (PMOS = 10, NMOS = 10)

### Voltage Transfer Characteristics

![VTC](PMOS10_NMOS10/vtc.png)

### Delay Analysis

![Delay](PMOS10_NMOS10/delay.png)

### Drain Current

![Drain Current](PMOS10_NMOS10/id.png)

---

## Configuration 2 (PMOS = 16, NMOS = 12)

### Voltage Transfer Characteristics

![VTC](PMOS16_NMOS12/vtc.png)

### Delay Analysis

![Delay](PMOS16_NMOS12/delay.png)

### Drain Current

![Drain Current](PMOS16_NMOS12/id.png)

---

## Configuration 3 (PMOS = 18, NMOS = 20)

### Voltage Transfer Characteristics

![VTC](PMOS18_NMOS20/vtc.png)

### Delay Analysis

![Delay](PMOS18_NMOS20/delay.png)

### Drain Current

![Drain Current](PMOS18_NMOS20/id.png)

---

# 🔍 Discussion

The electrical performance of the inverter changes with transistor sizing. Increasing the number of fins enhances the drive strength, resulting in higher drain current and slightly improved voltage gain. Although the propagation delay remains nearly constant for the configurations studied, the increase in transistor width also leads to higher dynamic power consumption.

These observations demonstrate the trade-off between performance and power in FinFET-based CMOS circuit design.

---

# ✅ Conclusion

A CMOS inverter was successfully designed and characterized using the ASAP7 7nm FinFET PDK. Three different PMOS/NMOS fin configurations were simulated and compared using Xschem and ngspice.

The extracted results show that transistor sizing significantly influences the inverter's electrical characteristics, particularly drain current, power consumption, threshold voltage, and gain. The study provides useful insight into sizing optimization for FinFET-based digital circuits and serves as a foundation for more complex standard-cell and memory circuit design.

---

# 📚 References

1. ASAP7 Predictive Process Design Kit (PDK)
2. BSIM-CMG Compact Model Documentation
3. Xschem User Manual
4. ngspice User Manual

---

# 👨‍💻 Author

**Param Sharma**

B.Tech Electronics and Communication Engineering

Manipal University Jaipur

