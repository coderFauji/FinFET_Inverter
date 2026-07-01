# CMOS Inverter Design with ASAP7 7nm FinFET PDK

---

### 1. Introduction

For decades, the semiconductor industry relied on flat, two-dimensional transistors known as planar MOSFETs to power microchips. However, as manufacturing technology advanced below the 22-nanometer (nm) node, these conventional transistors hit a physical barrier known as "short-channel effects."In a traditional planar transistor, the gate controls the flow of electricity through a channel from a single flat surface above. As transistors shrank, the source and drain terminals became too close together. The gate lost its physical grip over the channel, allowing electricity to leak through even when the transistor was turned "off." This resulted in massive power waste, high battery drain, and excessive heat generation.To overcome this bottleneck and sustain Moore's Law, the industry transitioned to a three-dimensional architecture: the Fin Field-Effect Transistor (FinFET).

<p align="center">
  <img src="Images/finfet_vs_mosfet.png" width="700">
</p>

---

## 1.1 Structural Design and Working Principle

The defining feature of a FinFET is its thin, vertical silicon body that projects upward from the chip substrate, resembling a fish’s fin. This vertical fin acts as the conducting channel connecting the source and the drain.Instead of sitting flat on top, the gate electrode wraps around the three exposed sides of this vertical fin (the left, top, and right sides). This "tri-gate" design fundamentally improves how the transistor functions:

- Superior Electrical Control: By surrounding the channel on three sides, the gate gains maximum electrostatic control over the electrons. This prevents the drain voltage from accidentally pulling electrons through when the device should be off.
- Elimination of Leakage Current: Because the vertical fin is extremely thin, no part of the channel is hidden far from the gate's influence. The gate can easily switch off the entire channel, shrinking idle power leakage.
- Increased Effective Channel Width (\(W_{eff}\)): In electrical engineering, a wider channel means more current can flow, making the device faster. In a planar layout, making a channel wider requires taking up valuable horizontal space on the chip. In a FinFET, the effective width is increased vertically using the height of the fin. For a single fin, the formula is:

\(W_{eff}=(2\times H_{fin})+W_{fin}\)

Engineers can boost power simply by building taller fins or placing multiple fins side-by-side, saving physical space on the microchip.

---

## 1.2 Key Advantages for Modern Electronics

FinFET technology provides several critical upgrades over older, flat transistor designs:

- Energy Efficiency: Because FinFETs successfully stop current leakage, they can operate at much lower supply voltages. This allows smartphones and laptops to run faster while consuming significantly less battery power.
- Faster Switching Speeds: The three-sided gate structure allows more current to rush through the channel when the transistor is turned "on," enabling processors to achieve higher clock speeds and processing frequencies.
- Better Manufacturing Reliability: At microscopic scales, minor imperfections during manufacturing can cause transistors to behave unpredictably. The tight control of the FinFET architecture reduces these variations, making modern microchips highly reliable.

---

## 2. Tools

To analyze sub-10nm transistor dynamics accurately, this project implements a fully open-source electronic design automation (EDA) software stack. The simulation framework integrates layout schematic capture, numerical SPICE execution, predictive process files, and industry-standard compact transistor model mathematics.

---

## 2.1 Xschem

Xschem is an open-source, graphic-driven schematic editor optimized for Very Large Scale Integration (VLSI) circuit design. Rather than writing thousands of lines of raw text code to define the inverter by hand, Xschem provides a visual canvas to wire the PMOS and NMOS symbols.Once drawing is complete, the software maps out the coordinates and electrical properties to generate a dedicated hardware netlist file. This translation turns visual symbols into mathematical net lists so the simulator can analyze the structural loops.

---

## How to install Xschem in ubuntu

```bash
git clone https://github.com/StefanSchippers/xschem.git xschem
cd xschem
./configure 
make 
sudo make install 
cd ..
```
---

## 2.2 Ngspice 

Ngspice is a completely free, open-source program based on SPICE3f5, a famous simulation software originally created at the University of California, Berkeley. The program allows users to simulate both analog and digital electronic circuits, testing everything from small individual transistors up to large, complex computing systems. Because it is highly flexible and includes a wide range of useful tools, ngspice is widely popular for checking and designing circuits in both college classrooms and professional industries. The software has several built-in features that make it an excellent tool for engineering students and designers. It can model many different kinds of electronic setups, from simple circuit loops with basic resistors and capacitors to highly advanced integrated circuits (microchips). When running a simulation, the software supports multiple types of standard engineering tests:
- DC Analysis: Sweeps voltage input levels up and down to check how the circuit behaves under steady conditions.
- Transient Analysis: Tracks how electrical signals change over time, which is critical for finding out exactly how fast a circuit switches.
- AC and Noise Analysis: Measures how the circuit responds to different electrical frequencies and checks for unwanted signal interference.

To make these calculations possible, ngspice includes a built-in library filled with mathematical definitions for basic parts like diodes, older transistors (such as BJTs and MOSFETs), and operational amplifiers. One of its best features is that it can read and simulate netlists (circuit text files) written for other commercial SPICE software programs, which makes moving project files back and forth very easy.In the electronics field, ngspice is typically used for three main purposes. First, hobbyists and engineers use it to build virtual prototypes, letting them catch mistakes and refine their designs before spending money on real physical parts. Second, professors use it as a teaching tool to give students hands-on experience with circuit design inside a safe, computer-simulated space. Finally, researchers use it to explore how experimental electronic components might behave in the real world. Users can also expand the program by writing their own custom control scripts or adding new transistor models to fit specific research needs. Because the software is constantly updated and improved by a global community of developers, it remains highly reliable and up-to-date.

---

## How to install Ngspice 

```bash
#Clone the ngspice source repository
git clone https://git.code.sf.net/p/ngspice/ngspice ngspice_git
#Move into the source directory
cd ngspice_git
#Create a separate build directory
mkdir release
#Generate the configure script
./autogen.sh
#Enter the build directory
cd release
#Configure ngspice with OSDI and XSPICE support
../configure --with-x --enable-xspice --disable-debug --enable-cider --with-readline=yes --enable-openmp --enable-osdi
#Build ngspice
make
#Install ngspice
sudo make install
#Verify installation
ngspice -v
```
---

## 2.3 ASAP_7NM

When designing real microchips, semiconductor factories (called foundries) provide engineers with a digital package of files called a Process Design Kit (PDK). This kit contains all the specific blueprints, dimensions, and manufacturing laws required to build a chip at a certain size. However, because commercial 7nm factory files are highly secret and locked behind strict corporate non-disclosure agreements, they are completely unavailable to students. To solve this problem, our project uses the ASAP7 (Arizona State Automated Platform 7nm) Predictive PDK. Developed by researchers at Arizona State University alongside ARM Research, ASAP7 is a completely free, highly realistic simulation kit created specifically for academic research and college cell design.The ASAP7 PDK works by taking real industrial manufacturing parameters and scaling them down into a predictive computer model that behaves just like a real factory kit. 

Installation of the above PDK can be done by cloning the git repo at https://github.com/The-OpenROAD-Project/asap7

The complete workflow followed during this project is illustrated below.


---

## 2.4 BSIM_CGS

To accurately model our inverter, the simulation environment must include the BSIM-CMG (Berkeley Short-channel IGFET Model for Common Multi-Gate structures) framework. Developed by the specialized Device Model Working Group (DMWG) at the University of California, Berkeley, this is an advanced mathematical blueprint designed specifically for simulating three-dimensional transistors like FinFETs and nanowire structures. While older computer models were built for simple flat transistors, BSIM-CMG expands on those original formulas to capture how electric fields behave in 3D space. It handles highly complex microscopic anomalies, including gate coupling (how different sides of the gate work together), short-channel effects, and quantum wave behaviors. Because of its precision, this model is universally trusted across both college research and professional factory engineering to optimize and predict the speed and power of modern nanoscale microchips.

To practically use the BSIM-CMG framework for simulating our 7nm inverter, we must adjust our SPICE netlist configuration files. Specifically, we change the model file extensions from .pm to .sp inside our setup parameters.

For researchers looking to download or study the absolute latest versions of these multi-gate transistor blueprints, the official repository is hosted publicly by the university at https://www.bsim.berkeley.edu/models/bsimcmg/.

Because these advanced models describe complex physical behaviors, they are written in a hardware programming language called Verilog-A (.va extension). Since circuit engines like ngspice cannot read raw Verilog-A code directly, an open-source compiler tool called **OpenVAF** is required to translate them. OpenVAF processes the .va files and converts them into a compact binary mathematical format called an **.osdi** file. Once compilation is complete, this .osdi file must be placed directly into your active working directory alongside your main .sp SPICE netlist. 
Note that only modern versions of ngspice support external .osdi plugins; older versions will fail to recognize them. 

To compile smoothly, make sure the OpenVAF tool is downloaded from https://openvaf.semimod.de/

---

## SIMULATION RESULTS

| S.No. | PMOS (nfin) | NMOS (nfin) | V<sub>TH</sub> (V) | I<sub>D</sub> (A) | Power (W) | Gain (A<sub>v</sub>) | g<sub>m,max</sub> (mS) | Delay (ps) | Frequency (GHz) |
|:-----:|:-----------:|:-----------:|:------------------:|:-----------------:|:---------:|:--------------------:|:----------------------:|:----------:|:---------------:|
| 1 | 10 | 10 | 0.345064 | 5.8599 × 10⁻⁷ | 2.1317 × 10⁻⁵ | 6.424 | 0.883 | 25.297 | 22.467 |
| 2 | 16 | 12 | 0.364919 | 7.0337 × 10⁻⁷ | 2.9242 × 10⁻⁵ | 6.441 | 1.284 | 25.459 | 22.286 |
| 3 | 18 | 20 | 0.337759 | 1.1719 × 10⁻⁶ | 4.0497 × 10⁻⁵ | 6.432 | 1.641 | 25.221 | 22.516 |

---


## Configuration 1 (PMOS = 10, NMOS = 10)

### SPICE Deck

```
** sch_path: /home/hprcse/Finfet/inverter_vtc.sch
**.subckt inverter_vtc
Xpfet1 nfet_out nfet_in vdd vdd asap_7nm_pfet l=7e-009 nfin=10
Xnfet1 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=10
V1 nfet_in GND pulse(529u 0.700529 20p 10p 10p 20p 500p 1)
V2 vdd GND 0.700529
Vuniq unique GND DC 0.529
**** begin user architecture code


.dc v1 0 0.700529 1m
*.tran 1e-12 100e-12
.control
    * First run DC
    dc v1 0 0.700529 1m
    run

    * DC measurements
    meas dc v_th when nfet_out = nfet_in
    plot nfet_out nfet_in
    
    let gain_av = abs(deriv(nfet_out))
    meas dc max_gain max gain_av
    let gain_target = max_gain * 0.999
    meas dc vil find nfet_in when gain_av = gain_target cross=1
    meas dc voh find nfet_out when gain_av = gain_target cross=1
    meas dc vih find nfet_in when gain_av = gain_target cross=2
    meas dc vol find nfet_out when gain_av = gain_target cross=2
    let nmh = voh - vih
    let nml = vil - vol
    print v_th max_gain vil voh vih vol nmh nml
    
    *Transconductance
    
    let id = v2#branch
    let gm = real(deriv(id, nfet_in))
    meas dc gm_max MAX gm
    plot gm
    let r_out= deriv(nfet_out,id)
    plot r_out
    plot id
    
    * Transient measurements
    tran 1e-12 100e-12
    meas tran tpr when nfet_in = 0.35 rise = 1
    meas tran tpf when nfet_out = 0.35 fall = 1
    let tp = (tpr + tpf) / 2
    let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=2e-11 to=6e-11
    let pwr = id_pwr * 0.700529
    let power = abs(pwr / 40e-12)
    print tpr tpf tp id_pwr pwr power
  
    tran 0.1 100p                         
    meas tran tr when nfet_in=0.07 RISE=1  
    meas tran tf when nfet_out=0.63 FALL=1 
    let t_delay = tr + tf                  
    print t_delay                         
    let f = 1/t_delay                     
    print f                              

    

.endc

**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_pfet S G D B l=7e-009 nfin=10
	npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=10
.ends asap_7nm_pfet

.model BSIMCMG_osdi_P BSIMCMG_va (
+ TYPE = 0

************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 3e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.9278          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2.5e-009       dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.8e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.003469        cdscd   = 0.001486        dvt0    = 0.05
+dvt1    = 0.36            phin    = 0.05            eta0    = 0.094           dsub    = 0.24
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 0
+etaqm   = 0.54            qm0     = 2.183e-012      pqm     = 0.66            u0      = 0.0237
+etamob  = 4               up      = 0               ua      = 1.133           eu      = 0.05
+ud      = 0.0105          ucs     = 0.2672          rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 60000           deltavsat= 0.17            ksativ  = 1.592
+mexp    = 2.491           ptwg    = 25              pclm    = 0.01            pclmg   = 1
+pdibl1  = 800             pdibl2  = 0.005704        drout   = 4.97            pvag    = 200
+fpitch  = 2.7e-008        rth0    = 0.15            cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 0               lcdscdr = 0               lrdsw   = 1.3             lvsat   = 1441
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.007           bigc    = 0.0015          cigc    = 1               dlcigs  = 5e-009
+dlcigd  = 5e-009          aigs    = 0.006           aigd    = 0.006           bigs    = 0.001944
+bigd    = 0.001944        cigs    = 1               cigd    = 1               poxedge = 1.152
+agidl   = 2e-012          agisl   = 2e-012          bgidl   = 1.5e+008        bgisl   = 1.5e+008
+egidl   = 1.142           egisl   = 1.142
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -1.2            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/param-sharma/project/vsd-7nm/asap_7nm_Xschem/bsimcmg.osdi
.endc



.subckt asap_7nm_nfet S G D B l=7e-009 nfin=10
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=10
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/param-sharma/project/vsd-7nm/asap_7nm_Xschem/bsimcmg.osdi
.endc


**** end user architecture code
.end
```

### Voltage Transfer Characteristics

<p align="center">
  <img src="INV_P10N10/vtc.png" width="1000">
</p>


### Delay Analysis

<p align="center">
  <img src="INV_P10N10/delay.png" width="1000">
</p>


### Drain Current

<p align="center">
  <img src="INV_P10N10/id.png" width="1000">
</p>


---

## Configuration 2 (PMOS = 16, NMOS = 12)

### SPICE Deck

```
** sch_path: /home/hprcse/Finfet/inverter_vtc.sch
**.subckt inverter_vtc
Xpfet1 nfet_out nfet_in vdd vdd asap_7nm_pfet l=7e-009 nfin=16
Xnfet1 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=12
V1 nfet_in GND pulse(529u 0.700529 20p 10p 10p 20p 500p 1)
V2 vdd GND 0.700529
Vuniq unique GND DC 0.529
**** begin user architecture code


.dc v1 0 0.700529 1m
*.tran 1e-12 100e-12
.control
    * First run DC
    dc v1 0 0.700529 1m
    run

    * DC measurements
    meas dc v_th when nfet_out = nfet_in
    plot nfet_out nfet_in
    
    let gain_av = abs(deriv(nfet_out))
    meas dc max_gain max gain_av
    let gain_target = max_gain * 0.999
    meas dc vil find nfet_in when gain_av = gain_target cross=1
    meas dc voh find nfet_out when gain_av = gain_target cross=1
    meas dc vih find nfet_in when gain_av = gain_target cross=2
    meas dc vol find nfet_out when gain_av = gain_target cross=2
    let nmh = voh - vih
    let nml = vil - vol
    print v_th max_gain vil voh vih vol nmh nml
    
    *Transconductance
    
    let id = v2#branch
    let gm = real(deriv(id, nfet_in))
    meas dc gm_max MAX gm
    plot gm
    let r_out= deriv(nfet_out,id)
    plot r_out
    plot id
    
    * Transient measurements
    tran 1e-12 100e-12
    meas tran tpr when nfet_in = 0.35 rise = 1
    meas tran tpf when nfet_out = 0.35 fall = 1
    let tp = (tpr + tpf) / 2
    let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=2e-11 to=6e-11
    let pwr = id_pwr * 0.700529
    let power = abs(pwr / 40e-12)
    print tpr tpf tp id_pwr pwr power
  
    tran 0.1 100p                         
    meas tran tr when nfet_in=0.07 RISE=1  
    meas tran tf when nfet_out=0.63 FALL=1 
    let t_delay = tr + tf                  
    print t_delay                         
    let f = 1/t_delay                     
    print f                              

    

.endc

**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_pfet S G D B l=7e-009 nfin=16
	npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=16
.ends asap_7nm_pfet

.model BSIMCMG_osdi_P BSIMCMG_va (
+ TYPE = 0

************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 3e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.9278          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2.5e-009       dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.8e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.003469        cdscd   = 0.001486        dvt0    = 0.05
+dvt1    = 0.36            phin    = 0.05            eta0    = 0.094           dsub    = 0.24
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 0
+etaqm   = 0.54            qm0     = 2.183e-012      pqm     = 0.66            u0      = 0.0237
+etamob  = 4               up      = 0               ua      = 1.133           eu      = 0.05
+ud      = 0.0105          ucs     = 0.2672          rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 60000           deltavsat= 0.17            ksativ  = 1.592
+mexp    = 2.491           ptwg    = 25              pclm    = 0.01            pclmg   = 1
+pdibl1  = 800             pdibl2  = 0.005704        drout   = 4.97            pvag    = 200
+fpitch  = 2.7e-008        rth0    = 0.15            cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 0               lcdscdr = 0               lrdsw   = 1.3             lvsat   = 1441
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.007           bigc    = 0.0015          cigc    = 1               dlcigs  = 5e-009
+dlcigd  = 5e-009          aigs    = 0.006           aigd    = 0.006           bigs    = 0.001944
+bigd    = 0.001944        cigs    = 1               cigd    = 1               poxedge = 1.152
+agidl   = 2e-012          agisl   = 2e-012          bgidl   = 1.5e+008        bgisl   = 1.5e+008
+egidl   = 1.142           egisl   = 1.142
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -1.2            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/param-sharma/project/vsd-7nm/asap_7nm_Xschem/bsimcmg.osdi
.endc



.subckt asap_7nm_nfet S G D B l=7e-009 nfin=12
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=12
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/param-sharma/project/vsd-7nm/asap_7nm_Xschem/bsimcmg.osdi
.endc


**** end user architecture code
.end
```

### Voltage Transfer Characteristics

<p align="center">
  <img src="INV_P16N12/vtc.png" width="1000">
</p>


### Delay Analysis

<p align="center">
  <img src="INV_P16N12/delay.png" width="1000">
</p>


### Drain Current

<p align="center">
  <img src="INV_P16N12/id.png" width="1000">
</p>


---

## Configuration 3 (PMOS = 18, NMOS = 20)

### SPICE Deck

```
** sch_path: /home/hprcse/Finfet/inverter_vtc.sch
**.subckt inverter_vtc
Xpfet1 nfet_out nfet_in vdd vdd asap_7nm_pfet l=7e-009 nfin=18
Xnfet1 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=20
V1 nfet_in GND pulse(529u 0.700529 20p 10p 10p 20p 500p 1)
V2 vdd GND 0.700529
Vuniq unique GND DC 0.529
**** begin user architecture code


.dc v1 0 0.700529 1m
*.tran 1e-12 100e-12
.control
    * First run DC
    dc v1 0 0.700529 1m
    run

    * DC measurements
    meas dc v_th when nfet_out = nfet_in
    plot nfet_out nfet_in
    
    let gain_av = abs(deriv(nfet_out))
    meas dc max_gain max gain_av
    let gain_target = max_gain * 0.999
    meas dc vil find nfet_in when gain_av = gain_target cross=1
    meas dc voh find nfet_out when gain_av = gain_target cross=1
    meas dc vih find nfet_in when gain_av = gain_target cross=2
    meas dc vol find nfet_out when gain_av = gain_target cross=2
    let nmh = voh - vih
    let nml = vil - vol
    print v_th max_gain vil voh vih vol nmh nml
    
    *Transconductance
    
    let id = v2#branch
    let gm = real(deriv(id, nfet_in))
    meas dc gm_max MAX gm
    plot gm
    let r_out= deriv(nfet_out,id)
    plot r_out
    plot id
    
    * Transient measurements
    tran 1e-12 100e-12
    meas tran tpr when nfet_in = 0.35 rise = 1
    meas tran tpf when nfet_out = 0.35 fall = 1
    let tp = (tpr + tpf) / 2
    let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=2e-11 to=6e-11
    let pwr = id_pwr * 0.700529
    let power = abs(pwr / 40e-12)
    print tpr tpf tp id_pwr pwr power
  
    tran 0.1 100p                         
    meas tran tr when nfet_in=0.07 RISE=1  
    meas tran tf when nfet_out=0.63 FALL=1 
    let t_delay = tr + tf                  
    print t_delay                         
    let f = 1/t_delay                     
    print f                              

    

.endc

**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_pfet S G D B l=7e-009 nfin=18
	npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=18
.ends asap_7nm_pfet

.model BSIMCMG_osdi_P BSIMCMG_va (
+ TYPE = 0

************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 3e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.9278          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2.5e-009       dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.8e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.003469        cdscd   = 0.001486        dvt0    = 0.05
+dvt1    = 0.36            phin    = 0.05            eta0    = 0.094           dsub    = 0.24
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 0
+etaqm   = 0.54            qm0     = 2.183e-012      pqm     = 0.66            u0      = 0.0237
+etamob  = 4               up      = 0               ua      = 1.133           eu      = 0.05
+ud      = 0.0105          ucs     = 0.2672          rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 60000           deltavsat= 0.17            ksativ  = 1.592
+mexp    = 2.491           ptwg    = 25              pclm    = 0.01            pclmg   = 1
+pdibl1  = 800             pdibl2  = 0.005704        drout   = 4.97            pvag    = 200
+fpitch  = 2.7e-008        rth0    = 0.15            cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 0               lcdscdr = 0               lrdsw   = 1.3             lvsat   = 1441
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.007           bigc    = 0.0015          cigc    = 1               dlcigs  = 5e-009
+dlcigd  = 5e-009          aigs    = 0.006           aigd    = 0.006           bigs    = 0.001944
+bigd    = 0.001944        cigs    = 1               cigd    = 1               poxedge = 1.152
+agidl   = 2e-012          agisl   = 2e-012          bgidl   = 1.5e+008        bgisl   = 1.5e+008
+egidl   = 1.142           egisl   = 1.142
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -1.2            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/param-sharma/project/vsd-7nm/asap_7nm_Xschem/bsimcmg.osdi
.endc



.subckt asap_7nm_nfet S G D B l=7e-009 nfin=20
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=20
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/param-sharma/project/vsd-7nm/asap_7nm_Xschem/bsimcmg.osdi
.endc


**** end user architecture code
.end
```

### Voltage Transfer Characteristics

<p align="center">
  <img src="INV_P18N20/vtc.png" width="1000">
</p>


### Delay Analysis

<p align="center">
  <img src="INV_P18N20/delay.png" width="1000">
</p>


### Drain Current

<p align="center">
  <img src="INV_P18N20/vtc.png" width="1000">
</p>


---


# 📚 References

1. ASAP7 Predictive Process Design Kit (PDK)
2. BSIM-CMG Compact Model Documentation
3. Xschem User Manual
4. ngspice User Manual

---

## Author

**Param Sharma**

B.Tech Electronics and Communication Engineering

Manipal University Jaipur

