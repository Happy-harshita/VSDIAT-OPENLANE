# VSDIAT-OPENLANE
# SKY130 Day 1: Inception of OpenSource EDA, OpenLANE and SKY130 PDK

## How to Talk to Computers

### Introduction to QFN - 48 package, chip, pads, core, die and IPs
A commonly and extensively used arduino circuit board can be seen in the below picture. The board has a processer or SoC i.e. System on a Chip. This is a central part of the chip and is encircled as can be seen below: 

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/c18a72f1-1e75-4fba-9bdc-fb5a51925941)

The encircled region, however is only a high level view, which can be represented through a block diagram, as shown below:

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/40e9d693-6e9f-4bb6-abb9-2e1500ba7358)

In the picture, we are able to see a large box which represents, as written a SoC. In layman's terms, this often called a "CHIP". However, in technicality it is termed as a "PACKAGE". One kind of package, which is used in the arduino board is a QFN - 48 package (Quad Flat No-Leads). A package can be schematically represented as below:

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/4fc5973d-d15c-49b1-9b2b-28af92892e02)

As seen, a chip is actually inside a package, and is connected to various "PINS" or inputs/outputs. The locations of the pins and what they are are usually driven by the design of the PCB. A chip is also a very complex system, and has various components such as -: 

* PADS : They act as connectors between the integrated circuit and chip. Pads, simply are structures for sending electrical signals inside the chip. They are strategically placed and made with respect to the pins
* CORE : This is the digital logic (i.e. the various gates such as AND, OR, XOR etc and the MUXes) is placed. It carries out all processor functions.
* DIE : A die is a part of semiconductor wafer that can be used independently in various devices. It can contain more than one core.

The core also has various components, which can mainly be segregated into -:

* FOUNDRY IPs : These are components that are provided to the designer of the SoC by the foundry (i.e. a factory where chips are produced). These components are usually pre-designed and tested by the foundry. Foundry IPs can include PLLs, adc0, adc1, SRAMs and various other components.
* MACROs : Macros are very similar to Foundry IPs, but consist of pure digital logic, that is pre-made for certain common use-cases and can be easily integrated into a SoC or Integrated Circuit (IC). Macros can be of various types such as a GPIO bank and SPI etc.

Two schematics encompassing all of the above components can be seen below:

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/a77af64a-c7ce-458b-b1ed-4979e9e78bd3)

### Introduction to RISC V
RISC-V Instruction Set Architechture, commonly called RISC V ISA, is the language of the computer. When a C program is to be run on a piece of hardware, it is first compiled in an assembly language program like RISC V. It is then converted to machine language i.e. binary and subsequently implemented in the form of a hardware description language such as picorv32 cpu core. A schematic is shown below, representing the same: 

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/4c76209c-d7c9-4a42-bdc4-96757c91a1c0)

### From Software Applications to Hardware
Application Software, or apps. They run on hardware such as laptops, mobile phones, tablets etc that are powered by chips. However, apps are coded in complex application programs that require conversion into binary or machine language for running. This is done through system software, which majorly consists of 

* Operating systems - Operating Systems (OSs) perform a variety of functions such as I.O. operation, memory allocation and low level system function. They produce functions in languages such as C, C++, VB and Java, which are sent to the compiler.
* Compilers - Compilers produce simple instructions (the format of which depends on the hardware) in the form of .exe documents, which are to the assemblers. These instructions are abstract interfaces between complex coding languages like C/C++ and hardware, and hence are called ISAs. They are known as the architechture of the computer.
* Assemblers - Assemblers convert the instructions from the compilers into binary, and the function is implemented.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/08efbb7e-3f62-4163-9252-32a16b5198ae)

## SoC Design and OpenLANE

### Introduction to Components of Opensource Digital ASIC Design
ASIC requires mainly three components for design
* RTL IPs - they are building blocks written in a hardware description language. These blocks describe the functioning of the circuit at a basic level and are pre designed and verified.
* EDA Tools - EDA, which expands to Electronic Design Automation tools are used for design, simulation and verification and the analyzing of circuit designs. Common tools are OpenSTA, OpenRoad etc.
* PDK data - Process Design Kit data is a collection of files used to model the fabrication process for EDA tools. It is usually provided by a foundry and include Process Design Rules, Device Models, Digital Standard Cell Libraries and IO libraries.

However, until June of 2020, there was no OPENSOURCE available PDK data, making it impossible to do ASIC design on opensource platforms. Nonetheless, this changed when Google and Skywater released PDK data on 130 nm chips.  There is a unfortunate common misconception that 130 nm is not up to the industrial mark, with chips reaching Armstrong sizes in today's date. This is debunked due to the following two reasons -:
- several applications do not require such advanced nodes, and 130nm chips provide a good amount of power for those cases
- fabrication costs are also much lesser for 130nm chips

130 nm chips are also not slow, as verified by intel and OSU-:

![image](https://github.com/user-attachments/assets/99c2652d-f92a-4379-ab9e-ae4b9e5ab99e)

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/d9d967c9-2ac1-42fe-978b-da8d1b4d32ed)

### Simplified RTL to GDS flow
The RTL to GDSII  ( Register Transfer Level to Graphic Design System II) design process takes many steps, that are -:

- Synthesis - it converts hardware description languages such as VERILOG into gate-level representations part of a standard cell library. The cells part of this library have a regular layout. Each of these have different views/models such as Electrical, HDL, Spice etc.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/dd0dd8fa-a04d-4de7-a318-275f8ec9d7f3)

- Floor Planning involves optimizing chip performance, area utilization, and connectivity through spatial arrangements (i.e. the layout and placement of various components)The three main purposes of floor planning are firstly, minimizing wire lengths, secondly, reducing signal delays, thirdly, optimizing power distribution, and fourthly, ensuring efficient chip utilization. Power Planning aims to ensure stable and reliable power delivery to all components by effective distribution and design of power supplies and power distribution networks (PDN). The main purposes of this are minimizing voltage drop and noise, reducing power distribution network (PDN) resistances and capacitances, and ensuring uniform power distribution throughout. Usually the chip is powered through VDD pads which are connected to various components through parallel rectangular strips causing lesser resistance.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/251ae097-c169-4fb3-a1af-99b5e400d89f)

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/c3a16ce6-5322-41d7-bfa9-8dedcde971e8)

Placement - it is the process of determing where a component will be placed on the chip. The components can include standard cells, macros, and I/O pads. The cells are usually placed on floorplan rows, and are aligned with the sites. There are majorly two steps - global and detailed.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/673f7f27-5d78-4cef-93be-7db872074f52)

Clock Tree Synthesis - This step is done before routing, because the clock needs to be routed by delivering the clock to all sequential elements.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/57940fb0-e2f4-47c1-ac1f-a616b0bcd7a4)

* Routing - The determination of the interconnections of the components throught the various metal layers, whose thickness, pitch etc is detailed by the PDK. The SKY130 has 6 layers.

* Sign Off - this majorly has verification through three processes:

         Design Rule Check (DRC) - this makes sure the design complies with manufacturing guidelines and is compatible for fabrication. It aims to detect and correct layout errors so that fabrication defects do not occur.

         Layout vs. Schematic (LVS) - here the layout is contrasted against the schematic to ensure consistency. LVS tools extract netlists and compare them for differences, after which the design proceeds to physical design flow

         Static Timing Analysis (STA) - it evaluates timing behaviour of a digital circuit to ensure design meets setup and hold time constraints, maximum clock frequency, and other timing requirements.

## Introduction to OpenLANE and Strive Chipsets
OpenLANE is an open-source digital ASIC jointly developed by efabless and Google, designed to automate the entire design process flow based on several components including OpenROAD, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. It aims to create a clean GDSII (i.e. no LVS violations, no DRC violations, and no timing violations) with no human intervention. striVe is family of SoCs created by Efabless.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/2251419d-e406-41e4-9e43-66ac0ea4ba03)
![image](https://github.com/user-attachments/assets/6a2632ce-594c-44f2-ac9a-b42f78daf862)

## ASIC Design flow.

![image](https://github.com/user-attachments/assets/ef0b7268-48d1-4034-8217-9ee95d3e8f7f)

* Synthesis Exploration - it generates a delay vs area report.
{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/3a6764f5-58ce-4308-848b-8cf0286f84e8)

* Design Exploration - sweeps design configuration and subsequently find best configuration for any given design. It produces a report as shown:

![image](https://github.com/user-attachments/assets/85d7e4a9-2dc5-42f7-9e36-b2e0b95183b0)

* OpenLANE Regression Testing
{IMAGE CREDITS - VSDIAT} 

![image](https://github.com/user-attachments/assets/ba40c7b1-75f7-4503-9c47-35bd7fd48580)

* Design for Test (also k/a DFT)
{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/670713c4-938f-442b-949a-f1153b6ee839)

* Physical verification (DRC & LVS) - Magic is used for DRC and Magic and Netgen for LVS.

* Logic Equivalence Check (LEC) - checks that the physical implementation and the netlist have the same logic. It is performed each time netlist is modified and checks that changing netlist did not change function.

* Dealing with Antenna Rules violations

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/d19ef831-927f-428a-9d44-d3e1e7e21581)

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/b789f5e7-c22d-43ff-84da-74b049b3a101)

* Timing Analysis (STA) - Here, the input also contains a synthesized netlist along with other data.

{IMAGE CREDITS - VSDIAT}

![image](https://github.com/user-attachments/assets/b0ffc24a-06f6-41a1-a087-5259fe47a8da)

Get Familiar to Opensource EDA tools
OpenLANE Directory Structure in Detail
Openlane is more of a flow than a tool comprising of the various opensource EDA tools such as Yosys, OpenSTA. The aim of openlane is to automate the entire RTL to GSII flow and make it clean and opensource. It is very similar to commercial EDA tools.

Exploring OpenSource directory through Linux terminal steps:-

* Open the virtual machine and then open terminal on it.

* Type cd Desktop and then cd work/tools to change directory to Desktop/work/tools, as this is where all openlane files are stored.

* After changing directory, type ls -ltr to list the contents of the directory.
Side note: ltr represents how the list of contents should be ordered . To find other ways, type ls --help.

* In this workshop, we are going to use openlane_working_dir, and hence we will change directory to it by typing cd openlane_working_dir. Afterwards, we list the contents using the same ls -ltr. In this workshop, we are going to use pdks and openlane directories and hence we will explore both in sequence.

* Starting with pdks, type cd pdks and then ls -ltr to view the contents in pdks. Here, we will explore sky130A, so similarly change directory and then view contents. One will observe two directories - libs.tech and libs.rif

* libs.tech contains all tool specific files. As seen in the picture - tools like Qflow, netgen, magic etc have directories. Opening the magic directory in libs.tech, we can see the following files:

* libs.ref contains all the technology/foundry related processes. Upon further exploration of sky130_fd_sc_hd, we see the following
cd .. reverses the directory one step behind to the parent directory.cd

* Next, we will open openlane

## Design Preparation Step
To open Openlane, we can use the docker command using interactive. After invoking the docker command, the prompt changes to bash-4.2$, and then one must type ls -lrth, and subsequently ./flow.tcl -interactive package require openlane 0.9 retrives all the required information for openlane.

OpenLane has various designs and we are most interested in picorv32a Inside the designs folder there is document named config.tcl which overrides the default settings. These configurations are design specific.(e.g. clock period, clock port, verilog files).

To setup the design of OpenLANE, we first need to prepare the design ensures that the final product functions correctly and reliably through prep -design picorv32a, which gives the following result.

The command when run, sets up a filesystem where the OpenLANE can store the results. This creates a folder inside the picorv32a directory which contains the command log files, results, and the reports dumped of the various tool. The folder will be only have the lef files generated by this design setup stage. The cell LEF files .lef and technology LEF files .tlef merge to generate merged.lef inside runs/tmp/, wherein a a folder with today's date will be created, inside which a tmp folder will have contents, and the merged.lef folder will contain the merged lef files.

Review Files After Design Prep and Run Synthesis
Opening the merged.lef file through the less command after design prep will give one a document as shown:

After this, type the command run_synthesis to run synthesis which converts an abstract netlist into a program to run yosys RTL synthesis, ABC scripts (for technology mapping) and openSTA. This process will take about 5-6 minutes, depending on system speed.

Steps to Charecterise Synthesis Results
After synthesis, under point 20, there are printing statistics, which can be used to calculate flip-flop ratio.

FLIP FLOP RATIO = NO OF DFFs / NO OF CELLS * 100

Solving this through our data, we get => 1613/18036*100 = 8.94%

After this, one may look at the results of the synthesis through hte picorv32a.synthesis.v file, which can be found as shown, and then viewed through the less command.

## Day 2

The first step in physical design is to define the width and height of the core and die: Beginning with a very simple netlist, that can extrapolated later we will first draw a basic diagram in the form of symbols that we will later convert into physical designs. We will take each cell (gates, specific cell like flip flop) and give it a standard (although rough for now) dimensions. As an example here, each unit will be 1 unit x 1 unit - i.e. 1 sq. unit in size, and since there are 4 gates/flip-flops here, the total size of the silicon wafer will 4 sq. units.

> NOTE : here, we are ignoring the wires

All logical cells will be placed inside the core - which is part of the die. If the logical cells occupy the core fully, it is known as 100% utilisation. Utilisation factor = Area occupied by netlist / Total area of core. In this case, we see that utilisation factor is 100%, but practically it is usually 50%. Aspect Ratio is the ratio between height and width. If the chip is square - it is 1, else the chip is rectangular in shape.

Utilisation factor = 50 %

Aspect Ratio = 2 : 4 = 1 : 2 = .5

### Concept of Pre-Placed Cells

Pre-Placed cells are complex logic blocks that can be reused. They are already implemented and cannot be touched by Auto Place and Route tools - and hence are required to be very well designed. Placement of such cells are user-based. A combinational logic - such as netlist shown does a particular function and is composed of various gates. We can divide this logic into blocks - while preserving the connectivity of the logic. By extending IO pins and making connections we can convert the logic into two parts - that are blackboxed and can be used as needed. If a design only requires a black box, it can be directly handed over to the designer with out much hassle. The various preplaced blocks available include memory, clock-gating cell, comparator, MUX. 
**The arrangement of these IPs in a chip are known as floorplanning.**

## De-Coupling Capacitors

We surround pre-placed cells with de-coupling capacitors. If we think of a circuit to be part of a block, whenever it is switched on there is a demand for current, which is supplied by the Vdd. Upon switching the circuit off, there is a discharge, which the ground accepts. However, practically when voltage is supplied is passes through a wire which causes it to reduce slightly due the resistance, inductance and capacitance in the wire, and the reduced voltage is called Vdd'. The Vdd' always needs to stay in the noise margin - which ranges from Vih to Voh. If this is not true, the circuit is unstable. This is due to the large physical distance between the actual voltage supply and the circuit.

Decoupling capacitors is a solution to this problem. Decoupling capacitors can be thought of as as a huge capacitor completely filled with charge. The equivalent voltage across the capacitor is same as across the main supply voltage. The capacitor decouples the circuit from the main supply. Hence, all the pre-placed cells get their power supply from the capacitors and hence are completely stable.

## Power Planning

Consider a particular piece of logic to be a _macro_, that is repeated many times on a single chip. It requires a lot of voltage, so voltage must be supplied through a decoupling capacitor. However, it is not feasible to add the de-coupling capacitors on the entire circuit - only critical elements can be decoupled.

If the 16 bit bus is connected to an inverter, then it means that all the capacitors will discharge the voltage at once. A lot of capacitors discharging at once cam cause **Ground Bounce** due to great amount of voltage needed to drained at the same time, and turning the capacitor on might cause **Voltage Drop** due to insufficient current. Ground bounce and voltage drop might cause the voltage to not be within the noise margin range. To solve this problem, we can have multiple powersource taps and sources ( known as a power mesh) where capacitors can source current from the nearest Vdd and sink current to the nearest Ground.

## Pin Placement and Logical Cell Placement Blockage

Take the above netlist as an example needing to be implemented. The information about the connections is coded through VERILOG (also known as VHDL).The input and output ports are placed left and right between the core and the die respectively. The placements of the ports is cell-specific. The clocks are continuously drive the cells and hence clock ports are bigger than data ports. Due to this, we also need the least resistance paths for the clocks. The size is inversely proportional to the resistance.

After the pin placement, we create Logical Cell Placement Blockage to ensure that the APR tool does not place any cell on the pin locations.

## Steps to Run Floorplan Using OpenLANE

1. The first step is setting the configuration variables - Before running floorplan, the configuration variables or switches must be set. These are present in **openlane/configuration**

The _README.md_ contains all configuration variables, which are segregated based on stage and the **.tcl** files consists of the default OpenLANE settings.

2. All _configurations/switches_ accepted by the current run are from **openlane/designs/[design - date]/config.tcl**

***

There is a order of priority -:

- _openlane/designs/[design-date]/sky130A_sky130_fd_sc_hd_config.tcl_
- _openlane/designs/[design]/config.tcl_
- _openlane/configuration/floorplan.tcl_

> In OpenLANE, it is important to note that the vertical and horizontal metals set one more than what we specify. For example, if the vertical metal is specified as 3, then it'll be 4.

3. Floorplan is to be run on OpenLANE through the command :- _run_floorplan_

### Review Floorplan Files and Steps to Review Floorplan

4. After running floorplan as above, it will produce a result that will be stored in the form of a design exchange format - and will contain the area of the Die as well as positions.The die area in this file is in database units and 1 micron is equivalent to 1000 database units. Area of die = (554570/1000) microns * (565290/1000) microns = 311829.1653 sq. µm.

### Review Floorplan Layout in Magic

5. The command _magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &_ should be typed to view the file.
6. Subsequently, press the **S** key to select the entire die and then **V** to center the view, and then **Z** to zoom. You will observe that the IO pins are placed equidistant to one another in a random mode as based on the configuration **(FP_IO_MODE = 1)** set in _openlane/configuration/flo
7. After this, typing _what_ on the **tkcon** window will give the layer of the selection.


## Library Binding and Placement
### Netlist Binding and Initial Place Design

+ The first step is to bind the netlist with physical cells i.e. cells with real dimension. The netlist contains various gates, that while in the schematic are of a certain shape as depicted, are usually square/rectangular in shape in production. These gates are given a specific shape, and in the end look very different from the netlist.

These blocks are sourced from a "_shelf_", known as a **library**. The library has cells with various shapes, dimensions and also contains information about the delay information. The library contains various sizes of cells with the same functionality too - since bigger cells have lesser resistance

+ The second step is **PLACEMENT**, which is done based on connectivity. As can be seen, flip flop 1 is close to the  _Din1_ pin and flip flop 2 is close to _Dout1_ pin. Combinational cells are placed in close proximity to FF1 and FF2 as to reduce delay.

### Optimise Placement Using Estimated Wire-Length and Capacitance

Here, we will estimate wirelength needed to connect the components together. If the wirelength is too long, we would need to install repeaters, as the signal may change over a long distance. Repeaters essentially recondition the same signal to it's prior strength.

### Congestion Aware Placement Using RePLACE

The command to run placement of OpenLANE - _run_placement_ is a wrapper which does three functions
- Global Placement (by using the RePlace tool) - there is no legalisation and HPWL reduction model is used
- Optimization (by Resier tool)
- Detailed Placement (by OpenDP tool) - legalisation occurs - where standard cells are placed in rows and there will be no overlap of the cells.

Placement aims to **converge the overflow value**. 

> NOTE: If placement will be sucessful and the designs will converge, the overflow value will progressively reduce during the placement.

After running the placement, output is generated in this folder **_openlane/designs/picorv32a/runs/[design - date]/results/placement/picorv32a.placement.def_**

Then, we can type the command : **_magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &_** to view it in Magic: 
    
## Cell Design and Characterisation Flows
### Inputs for Cell Design Flow and Circuit and Layout Design Step

Standard cells - for example AND gate, OR gate, BUFFER etc are stored in the _standard cell library_. There are various types of cells in the library with various variations as well - in drive strengths, functionality, and voltages. For a greater cell size, there is greater drive strength for longer wires. If there is high Vth, then it will take more time to switch than a lesser threshhold voltage cell.

The standard cell design flow is as follows:-

INPUTS (PDKS : DRC and LVS rules, SPICE models, library and user defined specs)

PROCESSES (circuit, layout design and charecterisation)

OUTPUTS (Circuit Description Language, GDSII, lef, timing, noise etc)

> DRC & LVS Rules contain tech files and poly substrate parameters
> 
> SPICE Models contain threshold, linear regions, saturation region equations with added foundry parameters, including NMOS and PMOS parameters
> 
> User defined specifications include cell height and cell width, supply voltage, pin locations, and metal layer requirement
> 
>  IMPORTANT: The standard cell library developer must adhere to the rules given by the foundry so that when the cell can be used on a real design without any errors
>
> Circuit design is done by modeling the pmos and nmos to meet input library requirement
> 
> Layout design is done using Euler's path and stick diagram on Magic layout tool


### Typical Characterisation Flow

Steps of Characterisation Flow:-

1) Reading of SPICE module files
2) Reading of netlist extracted by SPICE
3) Recognising buffer behaviour
4) Reading subcircuits
5) Attaching neccessary power sources
6) Applying stimulus
7) Provision of of neccessary output capacitance
8) Provision of simulation command

These steps are given to the CHARECTERISATION SOFTWARE KNOWN AS **GUNA** in the form of a configuration file, which will generate timing, noise and power models in the form of _.libs_ files.

## General Timing Characterisation Parameters
### Timing Threshhold Definitions

Here, we will talk about the semantics of the various _.libs_ files generated by GUNA. To do this, we will take this circuit as an example:

Here, the red line is output of first inverter and blue is output of second inverter.

### Propogation Delay and Transition Time

Propogation delay is calculated as = time(out_x_thr) - (time_x_thr). If the propogation delay is negative, it can cause quite unexpected results - as an output is generated before the input. Hence, threshhold values should be selected properly. Delay threshold is usually 50% and slew rate threshold is usually 20%-80%.

Transition time is calculated as = time(slew_high_x_thr) - time(slew_low_x_thr).
## Labs for CMOS Inverter NGSPICE Simulations
### IO Placer Revision

OpenLANE configurations can be changed inside the shell itself, on the fly. IO Mode is usually set to _random equidistant_. However, if we want to change this, we can do so through the following command typed after floorplan : **set ::env(FP_IO_MODE) 2**. After running this command, the IO [input - output] pins will not be equidistant in mode 2 (instead of the default - that is 1). 

After this, we may re-run floorplan, and then check by seeing that the pins are placed based on of Hungarian algorithms now i.e. stacked one over the other. 

> NOTE: changing the configuration on the fly will not change the runs/config.tcl, the configuration will only be available on the current session.


### SPICE Deck Creation For CMOS Inverter

The SPICE deck contains connectivity information about netlists, inputs to be provided, TAPS for the outputs etc. The component values are taken, that are usually -: for the PMOS it is .375u/.25u (i.e. the channel length is .25 micron and and the channel width is .375 micron). Ideally, the PMOS should be 2 to 3 times wider than the NMOS. This is as the PMOS hole carrier is slower than the NMOS carrier, and since the rise and fall time must be matched, to reduce the resistance, we increase the width of the PMOS. The next steps are to identify and name the nodes:

The syntax of the SPICE deck netlist PMOS and NMOS is _[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]_. It is to be noted that all components in a netlist are described based on its node and values.

> EXAMPLE SYNTAX - M1 OUT IN VDD VDD PMOS W=.375U L=.25u

### SPICE Simulation Lab for CMOS Inverter

The start of SPICE simulation is _.op_ where in Vin will be swept from 0 to 2.5 with 0.05V steps. The model file is **tsmc_025um_model.mod** that has all the technological parameters for the 0.25µm NMOS and PMOS.


For SPICE simulation, there are various steps-:

1) Open the NGSPICE simulator
2) Source the Circuit File through _source_ command
3) Execute it by the command _run_ and then use _setplot_ which allows one to view any plots possible from the simulations specified in the spice deck and will give you a choice for which simulation to be run
4) Then, type _display_ which will give you a choice of nodes to be plotted which when _plot out vs in_ is typed will be plotted on a graph.

### Switching Threshhold Vm

POINTS TO BE NOTED:

- The shapes of the graphs are almost the same, through which we can derive the conclusion that CMOS is a robust device
- The parameters that determine the robustness of the CMOS is the switching threshhold and the propogation delay

The Switching Threshhold is the point where the the input voltage is equal to the output voltage and both PMOS & NMOS are in saturation region. When these are turned on, there is a high chances of leakage and  that the current flows directly from VDD to GND. Due to this, short circuit can be seen.

### Static and Dynamic Simulation of CMOS Inverter

To find Vm, we use DC TRANSFER ANALYSIS. Simulation is essentially a sweep from 0V to 2.5V by taking 0.05V steps.

To find propogation delay, we use transient analysis when a pulse is applied to the CMOS.

### Lab Steps to GitClone VSDSTD Cell Design
The steps to gitclone are as follows:
1. Clone the custom inverter standard cell design from the github repository shared above
2. Clone the repository with the custom inverter design through the command _git clone https://github.com/nickson-jose/vsdstdcelldesign_
3. Subsequently, copy the tech file to the _vsdstdcelldesign_ directory (created through above step) by this command _cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/_
4. Then, open the custom inverter layout in MAGIC through this command: _magic -T sky130A.tech sky130_inv.mag &_cp__

## Inception of Layout and CMOS Fabrication Process

The 16 MASK CMOS Fabrication process is as follows:

### Create Active Regions

1. The first step is to select a substrate - which is where the entirety of your design is fabricated. The most common substrate is a P doped Silicon Substrate. A substrate is ideally lesser doped than it's wells.

2. The next step is creating an active region for transistors. It is to be noted that it is necessary to have isolation between the pockets, which can be done through
   - Growing 40nm of Silicon Dioxide
   - Depositing 80nm of Silicon Nitride.
   - Depositing a layer of photoresist
   - Deposit mask-1 layer on top of photoresist. It covers the photoresist layer that must not be etched away (protects the two transistor active regions)
   - Applying UV light to remove the layers on the unmasked regions
   - Removing mask-1 and photoresist layers
   - Placing the chip in the furnace to grow the oxide in other areas
   - Removing the Si3N4 layer using hot phosphoric acid to have only p-substrate and SiO2 left

### Formation of N and P well

3. P well and N well formation
    + Deposition of photo resist layer and define the areas to protect by deposition of mask-2 and 3. Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated and Mask 3 protects P-Well while N-Well is being formed
    + Application of UV Light to remove the exposed photoresist
    + Placing of chip in furnace to diffuse the boron and phosphorous to form wells. This process is called **Twintub** process.

> Boron [B] is used to form P-Well and Phosporus [P] is used to form N-well

### Formation of Gate Terminal

Gate Terminal is where Threshhold Voltage is controled - as seen below:

4. Formation of Gate
     + Deposit photo resist layer to define the areas to be protected, and then subsequently deposit mask-4. Then, UV light is applied, and the exposed area of photoresist is removed
     + Then, implantation of  low energy boron at the surface of p-well using mask-4 to control the threshold occurs
     + Similarly, implantation of phosphorous/arsenic for n-well using mask-5 occurs
     + Fixing the oxide which is damaged by implantation steps by removing extra SiO2 using the hydroflouric acid and re-grow high quality SiO2 on p-substrate to contol the oxide thickness occurs next
     + Addition of polysilicon film subsequently occurs
     + Then, mask-6 is added and etching using photolithography occurs
     + Then, mask 6 is etched off to form the gate terminal

### Lightly Doped Drain [LDD] Formation

5. LDD Formation - the reason LDDs are created is to prevent the hot electron which can eventually cause Si - Si bonds break or create voltage that passes the 3.2eV barrier leading to issues with doped regions. The second major need is to prevent another effect, known as the short channel effect which can cause gate malfunctioning due to the drain field penetrating the channel.
     + Mask 7  and 8 are created for NMOS (lightly doped N-type) and PMOS (lightly doped P-type) respectively.
     + Heavily doped impurity (N+ for NMOS and P+ for PMOS) are added for the actual source and drain but the lightly doped impurity which are also added help maintain spacing between the source and drain and prevent hot electron effect and short channel effect.
     + To protect the lightly doped regions, we also add SiO2 and create spacers using _plasma anisotropic_ etching

### Source and Drain Formation

6. Source and Drain Formation
     + Thin screen oxide is added to avoid channeling during. Channeling is when implantations dig too deep into substrate which is very problematic
     + We create Mask-9 is for N+ implantation and Mask-10 for P+ implantation
     + The side wall spacers maintain the N-/P- while implanting the N+/P+
     + High temperature annealing is done as well

### Local Interconnect Formation

7. Steps to Form Connects and Interconnects [LOCAL] - these are very important as they help in controlling the electrical charecteristics. These are also the only things accessible to the end user.
     + The thin screen oxide is removed for opening up the source, drain and gate for contact building. We use Titanium as it has less resistance.
     + Titanium Diselenide [Ti2Si2] is used for local interconnects
     + Mask 11 is formed and Titanium Nitride [Ti N] is etched off by RCA cleaning to create the first level contact

### Higher Level Metal Formation

8. Higher Level Metal Formation - These steps are very similar to the previous steps and are quite easy to understand.
     + The previous steps in the MASK process have created an uneven surface layer. A layer of Silicon Dioxide [SiO2] doped with phosphorous or boron -[boron reduces the temperature] [known as phosphosilicate glass and borophosphosilicate glass] is deposited on the wafer surface.
     + Then, the surface is polished using the CMP [Chemical Mechanical Polishing] technique to planarize the surface.
     + Contact holes are created through photolithography.
     + Various masks are used for the various processes after this:-
          - Mask 12 is created for the first contact holes
          - Mask 13 is used for the first Aluminum contact layer, which the contact holes are connected to.
          - Mask 14 creates the second contact holes
          - Mask 15 is similarly, for the second Aluminum contact layer
          - Finally, we use Mask 16 for making contact to topmost layer

### Lab Introduction to SKY130 Basic Layers Layout and LEF using Inverter
> In sky130A, the first layer is the LDD or local-i and then the m1, m2 layers and so on.
> The power and ground lines are located in m1.
> When polysilicon crosses ndiffusion then NMOS and if polysilicon crosses pdiffusion then PMOS is created.
> The output of the layout is the _LEF_ file, which is used by the router in APR to get the location of standard cell pins for proper routing. So it is essentially an abstract form of the layout of a standard cell.

### Lab Steps to Create STD Cell Layout and Extract SPICE Netlist

For the SPICE extraction of the custom inverter layout, we can enter the following commands in the TCKON -:
1. _**extract all**_
2. _**ext2spice cthresh 0 rthresh 0**_ --> This extracts the parasitic information
3. _**ext2spice**_

## SKY130 Tech File Labs
### Lab Steps To Create Final SPICE deck using SKY130 tech

The default SPICE deck file using Sky130 is seen in the previous section 👆. Now, we can modify the file to plot a transient response, which would then create a final SPICE deck file by editing as below.

To load the SPICE file for simulation in NGSPICE, type the following command : _**ngspice sky130A_inv.spice**_

### Lab Steps to Characterise Inverter using SKY130 Model Files

After typing this command you will get outputs.

Using this, we can characterise the cell through four parameters - 
1. value of rise transition, which is the time taken for an output waveform to transit from a value of 20% of the maximum value to 80% of the maximum value = vdd = 3.3V. 20% of 3.3V is about .66V, and 80% is about 2.64V, and hence we will click on those points, whose x and y values will appear in the terminal.
   Then, rise transition = 2.2457ns - 2.1819ns = 0.638
   
2. value of fall transition, which is the time taken for an output waveform to transit from a value of 80% to 20% of the maximum value. Similarly, 4.06818ns - 4.04073ns = 0.02745ns

3. value of fall cell delay, which is the time difference {delay} between 50% of the input and 50% of the output which essentially means the time taken for output to fall to 50% and time taken for input to rise to 50%. Calculating fall delay => 4.05402ns - 4.0501ns = 0.00392ns

4. value of rise cell delay, which is the time difference {delay} between 50% of the input and 50% of the output which essentially means the time taken for output to rise to 50% and time taken for input to fall to 50%. Calculating rise delay => 2.18381ns - 2.15003ns = 0.03378ns

Through this, we have charecterised a cell at 27 degrees Celsius successfully -:
[cell was plotted with analysis occuring at 27 degrees Celsius]

### Lab Introduction to Magic Tool Options and DRC Rules

A documentation shared by the instructor on how MAGIC performs DRC [Design Rule Check] and it's syntax for the rules is at [this link](http://opencircuitdesign.com/magic/) 

### Lab Introduction to SKY130 PDKs And Steps to Download Labs
Now, for the lab we need to download the lab files, which can be done through this command -: _**wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz**_

Then, to extract these files, we can type _**tar xfz drc_tests.tgz**_

Then we will go into the directory by **_cd drc_tests_** and then list the contents through _ls -al_. Then to open the magic tool, type _**magic -d XR**_

### Lab Introduction to Magic and Steps to Load SKY130 Tech Rules

Then, in the empty magic prompt, click **"FILE"**, which is at the top and select _open_ and then _met3.mag_ under that, to obtain this screen view wherein we can see a number of independent layouts contain certain DRC errors

> NOTE: the text in purple is the periphery rules broken

### Lab Excercise to Fix **poly.9** error in SKY130 Tech-File

Type **_load poly_** in the tkcon window -:

Now, to focus on the poly 9 rule as explained at [this link](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root) which states that poly resistor spacing to poly or spacing (no overlap) to diff/tap should be atleast 0.48um! Now, we can look at the below screenshot to find that this is not true, here and hence must be fixed by changing the tech file to include this DRC.

Firstly, open the **_sky130A.tech_** file from the directory _drc_tests_. The rules included for <u>poly.9</u> are only for the spacing between the n-poly and p-poly resistor with diffusion. So, we will now add new rules for the spacing between the poly resistor with poly non-resistor. Highlighted in green below are the two newly added rules. First one is the rule for the spacing between the p-poly resistor with poly non-resistor and the next one is the rule for spacing between n-poly resistor with poly non-resistor. The allpolynonres is a macro under alias section of techfile.

### Lab Excercise to Implement Poly-Resistor Spacing to Diff and Tap

In the below pic, we see an error which we can fix by modifying the tech file by not including only the spacing between npolyres with N-substrate diffusion in poly.9 but also between npolyres and all types of diffusion.

Timing Modelling Using Delay Tables
Lab Steps To Convert Grid Information Into Track Information
sky130_inv.mag located in vsdstdcelldesign directory contains all information like PG and port information, logic etc. OpenLANE is a PnR tool and hence does not require the full information present in the .mag file. The only information that we require are the boundary, power and ground rails, and the inputs & outputs information. This is the reason of we use .lef files. Hence, our next objective is to extract the LEF file from the MAGIC file and plug that into the picorv32a design. The guidelines to be followed while making a standard cell are -:

The input and output ports must lie at the intersection of the horizontal and vertical tracks (this is to ensure the routes can reach the ports).
The width and height of the standard cell must be odd multiples of the track's horizontal and vertical pitch respectively
Tracks refer to the horizontal and vertical metal layers on which routing occurs. The grid formed by the intersection of horizontal and vertical tracks creates a routing grid, also k/a a routing matrix.

The ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info contains track information.

Lab Steps to Convert Magic Layout to STD Cell LEF
LEF [Library Exchange Format] which contains information of the standard cell library used in the design. They contain detailed information about the geometric shapes, sizes, layers, and other physical properties of individual cells or macros within the library. The instructions to set the port definitions are available here. Next, save the .mag file with a new filename by typing lef write in the tkcon terminal, which will generate a new lef file.

Introduction of Timing libs and Steps To Include New Cell in Synthesis
Inside this directory [pdks/sky130A/libs.ref/sky130_fd_sc_hd/lib/] are the liberty timing files for SKY130 PDK which have the timing and power parameters for each cell needed in STA. It can either be slow, typical, fast with different supply voltages (1v80, 1v65, 1v95), which are called PVT corners. The library named sky130_fd_sc_hd__ss_025C_1v80 describes the PVT corner as slow-slow [ie. delay is maximum], 25° Celsius temperature, at 1.8V power supply. Timing and power parameter of a cell are obtained by simulating the cell in a variety of operating conditions [i.e. different corners] and this data is represented in the liberty file, which characterizes all cells and is used during ABC mapping during synthesis stage which maps the generic cells to the actual standard cells available in the liberty file.

Firstly, copy the extracted lef file - named sky130_vsdinv.lef and the liberty files named sky130.lib* from this repository - /openlane/vsdstdcelldesign/libs to the src directory of picorv32a.

Subsequently, add the following to the config.tcl file inside the picorv32a directory. Through this, we are setting the liberty file that will be used for ABC mapping of synthesis (LIB_SYNTH) and for STA (_FASTEST,_SLOWEST,_TYPICAL) and also the extra LEF files (EXTRA_LEFS) for the customized inverter cell.

After this, invoke the docker command and prepare the picorv32a design. Plug the new lef file to the OpenLANE flow through the following commands

docker

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

Then, run synthesis using the run_synthesis command and check that sky130_vsdinv cell is successfully included in the design

The lesson tells us how the enable pin affects the CLK propogation. In case of AND gate, ONLY when the enable pin is equal to 1, the CLK propogates to Y. Similarly, in case of OR gate, the CLK propogates to Y only when the enable pin is 0. Whenever the enable pin is equal to 1, the CLK does not propogate and there is no short circuit power consumption. Switching power consumption when such elemements are used is used in CLOCK TREE. This method is known as Clock Gating Technique.

However, when we think of this, the question that arises is HOW DOES ONE USE THIS? For this purpose, consider the below clock tree structure -:

We see through the above picture that buffers on different levels have different capacitive loads and buffer sizes but as long as they have the same loads and sizes in the same level, the total delay for each clock tree path will be the same and, so skew will remain zero. However, we can observe that practically, different levels can have varying input transition and output capacitive load and hence varying delay.

We use delay tables to observe each cell's timing models that is included inside the .lef files. The element that majorly impacts delay is output slew, which depends on capacitative load and the input slew [which is a function of previous stage buffer's output capacitive load and input slew and has its own transition delay table.]

We notice at level 2 that both buffers have identical delays due to equal transition times, load capacitances and buffers sizes. Subsequently, we see that the skew is zero. However if this is not true, the skew will be negative, which will cause timing violations. On a small scale, these are often considered to be insignificant but we see their importance in large designs which millions of cells, where in if we fail to adhere to guidelines during clock tree creation, many timing-related complications can be caused.

CTS is the process of designing a clock distribution network to minimize skew and ensure synchronous operation of the circuit

Skew refers to the variation in clock signal arrival times, and slew [rate] is the rate of change of signal's voltage on time

Latency is the delay experienced by the clock signal

Lab Steps To Configure Synthesis Settings to Fix Slack and Include VSDINV
We can see through previous pictures that currently,

tns = -711.59
wns = -23.89
chip area for the module picorv32a = 147712.9184
Hence, our next step is to try to make the synthesis more timing driven. This can be done as follows-:

Firstly, check synthesis strategy and other timing related variables and then modify accordingly
SYNTH_STRATEGY of delay 0 will mean that the tool will focus more on optimizing the delay
index can changed to be 0, 1, 2, or 3 where 3 is the most optimized for timing at the cost of area.
SYNTH_BUFFERING of 1 will ensure that the buffer will be used on high fanout cells to reduce wire delay.
SYNTH_SIZING of 1 will enable cell sizing where cell will be upsized or downsized as needed to meet timing.
SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength.

After this, run synthesis again. It is will be seen that area is increased but there is no negative slack
tns = 0
wns = 0
Chip area for module picorv32a = 209181.872

Subsequently, we may run floorplan and placement
NOTE : If any error comes related to macro placement, temporary solution is to comment basic_macro_placement inside the file OpenLane/scripts/tcl_commands/floorplan.tcl (this is okay since we are not adding any macro to the design).

We can also execute the following commands:-

init_floorplan

place_io

global_placement_or

detailed_placement

tap_decap_or

detailed_placement

After successful run, runs/[date]/results/placement/picorv32a.placement.def will be created.

After this, search for instance of cell sky130_vsdinv inside the DEF file after the placement stage through this command cat picorv32a.placement.def | grep sky130_vsdinv

Then, select a single sky130_vsdinv cell instance from the list dumped by grep (e.g. 41096). On tkcon, command select cell 41096 may be run. Press ctrl+z to zoom into that cell. As shown below, our customized inverter cell sky130_myinverter is sucessfully placed. Use the expand command on tkcon to show the footprint of the cell and notice how the power and ground of sky130_vsdinv overlaps the power and ground pins of its adjacent cells.

Timing Analysis With Ideal Clocks Using Open STA
Setup Timing Analysis And Introduction to Flip-Flop Setup Time
Consider an ideal clock [i.e. the clock tree is not built yet] where perform timing analysis to understand the parameters [later the same can be done using real clocks]. Specifications are as mentioned in the picture [Clock frequncy (F) is 1GHz and clock period (T) is 1ns].

The setup timing analysis equation is = Θ < T - S

Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop

T = Time period, also called the required time

S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.

Introduction to Clock Jitter and Uncertainty
CLK signals are sent to the device by the PLL (Phase Locked Loop). This clock source is expected to send clock signal at 0, T, 2T etc. However, even these clock sources might or might not be able to provide a clock exactly at Tns because of its own in-built variation known as jitter. Jitter can be thought of as short-term fluctuations in the timing of signal transitions, which can result in deviations from the expected clock or data timing.

Hence, we see that a more realistic equation for setup time is = Θ < T - S - SU

SU = Setup uncertainty due to jitter which is temporary variation of clock period, which is due to non-idealities of PLL.

Lab Steps to Configure OpenSTA for Post-Synth Timings Analysis
STA can either be -:

single corner - which only uses the LIB_TYPICAL library which is the one used in pre-layout(pos-synthesis) STA
multi corner - multicorner which uses LIB_SLOWEST(setup analysis, high temp low voltage),LIB_FASTEST(hold analysis, low temp high voltage), and LIB_TYPICAL libraries
In the location *Desktop/Work/tools/openlane_working_dir/openlane_ there is a file named pre_sta.conf on which we will be doing the prelayout analysis. The file contains the following contents-:

This is the SDC file on which analysis will be done is located at Desktop/Work/tools/openlane_working_dir/openlane/designs/picorv2a/src and is named *my_base.src_. 

The various commands to be run and their functions are -:

create_clock -it creates clock for the port with specified time period.

set_input_delay and set_output_delay - defines the arrival/exit time of an input/output signal relative to the input clock. [This is the delay of the signal coming from an external block and internal delay of the signal to be propagated to external ports] This adds a delay of Xns relative to clk to all signals going to input ports, and delay of Yns relative to CLK to all signals going to output ports.

set_max_fanout - specifies maximum fanout count for all output ports in the design.

set_driving_cell - models an external driver at the input port of the current design.

set_load sets a capacitive load to all output ports.

Execute sta pre_sta.conf and check timing.

Lab Steps to Optimize Synthesis to Reduce Setup Violations
To reduce negative slack, focus on large delays. Net with big fanout might cause delay increase. Use report_net -connections <net_name> to display connections. First thing we can do is to go back to OpenLane and reduce fanouts by setting ::env(SYNTH_MAX_FANOUT) 4 then run_synthesis again.

For reducing the negative slack obtained, we must focus on the large delays and try to reduce them. Increases in delays are majorly caused by nets with big fanouts, which result in high load cap which causes high delay. To display the connections of the nets, the commands report_net -connections <net_name> can be used. Now, looking upon this output, we realise that fanouts must be reduced. This can be done by going back to OpenLANE and reducing fanouts by typing ::env(SYNTH_MAX_FANOUT) 4. After this, we can run_synthesis again and obtain a expected value for slack.

Lab Steps to do Basic Timing ECO
However, if we want to reduce the negative slack even more, we can upsize the cells with high fanouts so that bigger drivers will be used. Now, since we cannot change load capacitance but can change the cell size, we can increase the size of the cell for a large driver to drive large cap loads leading to lesser delay. This is often done in iterations until we reach the desired slack in a process known as Timing ECO [Engineering Change Order]

Clock Tree Synthesis TritonCTS and Signal Integrity
Clock Tree Routing and Buffering Using H-Tree Algorithm

Take into account the CLK port going to the various flip-flops in the above picture. It's purpose is to connect the port to the CLK pins of the many flip-flops based on the connectivity information given. In the above picture, we have blindly made connections and hence t2 is greater than t1. Skew is the difference between t2 and t1. CLK skew, as explained earlier, refers to the variation in arrival times of the clock signal at different points within a digital system. More simply, CLK skew is essentially the difference in propogation delay of the CLK signal as it moves along various paths. It can occur due to -:

differences in wire lengths
variations in signal routing paths
variations in buffer delays
other physical and environmental factors
Due to this, some parts of the system can recieve CLK signals earlier than or after the rest of the system. Minimizing clock skew is essential as to -:

ensure proper synchronization of signals
reliable operation of the digital circuit.
NOTE : Ideally, the skew should be zero.

Now, since the skew is not minimum, or even close to minimum in the above picture, we can call this tree a bad tree. To optimize this scenario, we can use H-Tree routing. It analyses the clock route by calculating the distance from the source to all the endpoints and deciding on a midpoint to start building tree. Subsequently, it finds another midpoint and creates divergents in the wire. Eventually, after repeating this process, the CLK reaches all the flip-flops at almost the same time, while splitting up at various midpoints.

It is expected that the input CLK signal is exactly reproduced at the output. However, this does not happen in H-Tree routing due to inherent resistance and capacitance in physical wires, which may cause the signal to experience distortion. We can solve this problem by inserting buffers/repeaters along the path for signal integrity. In the past, we have also talked about another kind of repeaters, used in data paths. The key differences between repeaters used in clock and data paths respectively lie in their rise and fall times. Clock buffers have same rise and fall times, to ensure uniform signal propagation throughout the clock distribution network. Contrastingly, data buffers exhibit varying rise and fall times, which may differ based on the characteristics of the data being transmitted and the components involved in processing it.

Crosswalk and Clock Net Shielding
Clock nets are critical nets in the design because clock tree is built is such a fashion that the skew is zero. There is a phenomenon called crosstalk where a signal transmitted on one channel interacts with or interferes with signals on adjacent channels leading to distortion, noise, timing errors etc, which can cause the clock tree structure will be deteriorated. To solve this, all the clock nets are shielded [protected]. If there is a wire adjacent to such shields, then there exists a huge coupling capacitance causing two issues - (i) glitch and (ii) delta delay.

Whenever there is a switching activity happening in the aggressor, then the coupling capacitance is so strong that it directly affects the net close to it named the victim net, which is without any shielding. This causes a dip in the voltage, resulting in glitch. Due to a glitch, there can be incorrect data in memory, which can cause inaccurate functionality. To solve this, we do shielding which protects the victim nets by breaking the coupling capacitance between the aggressor and the victim. These shielding nets are either in the form of Vdd or Vss. The shields do not switch, and hence the victim will not switch as well.

Lab Steps to run CTS using TritonCTS
After completion of Timing ECO, we see that the timing currently is still above 1. To reduce it, we can go through the results and switch the cells causing a lot of slack to buffers. After this the negative slack is reduced to below 1.

After this, for OpenLANE to use the modified netlist, we can type write_verilog [filename] and then run floorplan and placement.
After running CTS, we can see that the clock buffers get added.

Lab Steps to Verify CTS Runs
After the previous step, OpenLANE will take the procedures from /Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands and eventually invoke OpenROAD to run the tool.

For example, the command run_cts is found in the location /OpenLane/scripts/tcl_commands/cts.tcl. This tcl process will invoke OpenROAD which will call the cts.tcl which contains the OpenROAD commands for TritonCTS.

The file contains many configuration variables for CTS like-:

CTS_CLK_BUFFER_LIST -> which has the list of clock buffers used in clock tree branches (sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8)
CTS_ROOT_BUFFER -> this is the clock buffer used for the root of the clock tree and is the biggest clock buffer to drive the clock tree of the whole chip (sky130_fd_sc_hd__clkbuf_16)
CTS_MAX_CAP -> it is a numerical value for the maximum capacitance of the output port of the root clock buffer
Timing Analysis With Real Clocks Using Open STA
Setup Timing Analysis Using Real Clocks
Now the clock tree is built and timing analysis is done on real clocks.

delta1 = launch flop clock network delay

delta2 = capture flop clock delay

Any design satisfying Slack [i.e. Data required time - Data arrival time] is ready to work in the given frequency. However, if this equation is violated, then slack will become negative which is not expected. We expect slack to be either zero or positive.

Hold Timing Analysis Using Real Clocks
Hold Time is delay [time] needed by the MUX2 model within a flip-flop to transfer certain data outside. [i.e. how long it holds the data]. It is the time period during which the launch flop must retain dat before it reaches the capture flop. Unlike setup analysis, which has two rising clock edges, hold analysis occurs on the same rising clock edge for both the launch and capture flops.

A hold violation occurs when the path is too fast, impacted by factors such as -:

combinational delay
clock buffer delays
hold time.
Notably, parameters such as time period and setup uncertainty hold no significance, as both launch and capture flops receive identical rising clock edges during hold analysis.

Lab Steps to Analyse Timing With Real Clocks Using OpenSTA
The aim is to analyse the clock tree explained previously. The steps for analysis of timings with real clocks are -:

Enter into OpenROAD using the openroad command.
NOTE: In OpenROAD, timing is done slightly differently, where in a db is created from lef and def files and subsequently used

Subsequently create the db file through this command -: *read_lef [location of merged.lef]

Then, we must read the def file from the CTS stage

Subsequently create the db through this command -: write_db pico_cts.db

Then, read the db, verilog files, library and sdc.

After this, set the propogated clock to calculate the actual delay in the clock path through the command -: set_propogated_clock [all_clocks]

Subsequently check the timings -:

Lab Steps to Excecute OpenSTA With Right Timing Libraries and CTS Assignment
TritonCTS is build to optimise only based on one corner. However, the libraries included previously that also take part in timing analysis optimise based on both min and max corners, which is not accurate. Hence, we need to exit and re-enter OpenROAD and then check timing only for typical corner.

Here, we can see that slack is met for both setup and hold analysis in the typical scenario-:

NOTE: When the CTS is built, skew values are tried to be met by inserting buffers from CTS_CLK_BUFFER_LIST, which can be modified based on requirements.

When TritonCTS builds the clock tree, it tries to use each buffer listed in $::env(CTS_CLK_BUFFER_LIST) (sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8) from smallest to largest until the target skew is met.

It stores target skew in $::env(CTS_TARGET_SKEW).

The STA result also shows that sky130_fd_sc_hd__clkbuf_1 is the most used buffer, and we can also change the $::env(CTS_CLK_BUFFER_LIST) to use other buffers and observe the effect on STA and area.

We can also use tcl lreplace command to modify $::env(CTS_CLK_BUFFER_LIST)

Example:

The $::env(CURRENT_DEF) used by CTS is the DEF file of the previously run CTS, but the DEF file we want for CTS is the placement's DEF file. So to implement this, change the $::env(CURRENT_DEF) to point to placement DEF file then run_cts.

Now, observe the resulting post-CTS STA compared to previous run since we modified the clock buffer. Only buf_2 clock buffer is used now compared to buf_1 used in previous run, and hte WNS is better now since we have used bigger clock buffers.

Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/DAY 4.md at main · Happy-harshita/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah Timing Modelling Using Delay Tables
Lab Steps To Convert Grid Information Into Track Information
sky130_inv.mag located in vsdstdcelldesign directory contains all information like PG and port information, logic etc. OpenLANE is a PnR tool and hence does not require the full information present in the .mag file. The only information that we require are the boundary, power and ground rails, and the inputs & outputs information. This is the reason of we use .lef files. Hence, our next objective is to extract the LEF file from the MAGIC file and plug that into the picorv32a design. The guidelines to be followed while making a standard cell.

The input and output ports must lie at the intersection of the horizontal and vertical tracks (this is to ensure the routes can reach the ports).
The width and height of the standard cell must be odd multiples of the track's horizontal and vertical pitch respectively
Tracks refer to the horizontal and vertical metal layers on which routing occurs. The grid formed by the intersection of horizontal and vertical tracks creates a routing grid, also k/a a routing matrix.

The ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info contains track information. Before and After changing the grid values -:

Lab Steps to Convert Magic Layout to STD Cell LEF
LEF [Library Exchange Format] which contains information of the standard cell library used in the design. They contain detailed information about the geometric shapes, sizes, layers, and other physical properties of individual cells or macros within the library. The instructions to set the port definitions are available here. Next, save the .mag file with a new filename by typing lef write in the tkcon terminal, which will generate a new lef file with the new filename -:

Introduction of Timing libs and Steps To Include New Cell in Synthesis
Inside this directory [pdks/sky130A/libs.ref/sky130_fd_sc_hd/lib/] are the liberty timing files for SKY130 PDK which have the timing and power parameters for each cell needed in STA. It can either be slow, typical, fast with different supply voltages (1v80, 1v65, 1v95), which are called PVT corners. The library named sky130_fd_sc_hd__ss_025C_1v80 describes the PVT corner as slow-slow [ie. delay is maximum], 25° Celsius temperature, at 1.8V power supply. Timing and power parameter of a cell are obtained by simulating the cell in a variety of operating conditions [i.e. different corners] and this data is represented in the liberty file, which characterizes all cells and is used during ABC mapping during synthesis stage which maps the generic cells to the actual standard cells available in the liberty file.

Firstly, copy the extracted lef file - named sky130_vsdinv.lef and the liberty files named sky130.lib* from this repository - /openlane/vsdstdcelldesign/libs to the src directory of picorv32a.

Subsequently, add the following to the config.tcl file inside the picorv32a directory. Through this, we are setting the liberty file that will be used for ABC mapping of synthesis (LIB_SYNTH) and for STA (_FASTEST,_SLOWEST,_TYPICAL) and also the extra LEF files (EXTRA_LEFS) for the customized inverter cell.

After this, invoke the docker command and prepare the picorv32a design. Plug the new lef file to the OpenLANE flow through the following commands
docker

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

This will generate the following picture -:

Then, run synthesis using the run_synthesis command and check that sky130_vsdinv cell is successfully included in the design

NOTE : here, timing is not fixed, which then needs to be fixed.


Delay Tables

The image depicts to us how the enable pin affects the CLK propogation. In case of AND gate, ONLY when the enable pin is equal to 1, the CLK propogates to Y. Similarly, in case of OR gate, the CLK propogates to Y only when the enable pin is 0. Whenever the enable pin is equal to 1, the CLK does not propogate and there is no short circuit power consumption. Switching power consumption when such elemements are used is used in CLOCK TREE. This method is known as Clock Gating Technique.

However, when we think of this, the question that arises is HOW DOES ONE USE THIS? For this purpose, consider the below clock tree structure -:

We see through the picture that buffers on different levels have different capacitive loads and buffer sizes but as long as they have the same loads and sizes in the same level, the total delay for each clock tree path will be the same and, so skew will remain zero. However, we can observe that practically, different levels can have varying input transition and output capacitive load and hence varying delay.

We use delay tables to observe each cell's timing models that is included inside the .lef files. The element that majorly impacts delay is output slew, which depends on capacitative load and the input slew [which is a function of previous stage buffer's output capacitive load and input slew and has its own transition delay table.]

We notice at level 2 that both buffers have identical delays due to equal transition times, load capacitances and buffers sizes. Subsequently, we see that the skew is zero. However if this is not true, the skew will be negative, which will cause timing violations. On a small scale, these are often considered to be insignificant but we see their importance in large designs which millions of cells, where in if we fail to adhere to guidelines during clock tree creation, many timing-related complications can be caused.

CTS is the process of designing a clock distribution network to minimize skew and ensure synchronous operation of the circuit

Skew refers to the variation in clock signal arrival times, and slew [rate] is the rate of change of signal's voltage on time

Latency is the delay experienced by the clock signal

Lab Steps To Configure Synthesis Settings to Fix Slack and Include VSDINV
We can see through previous pictures that currently,

tns = -711.59
wns = -23.89
chip area for the module picorv32a = 147712.9184
Hence, our next step is to try to make the synthesis more timing driven. This can be done as follows-:

Firstly, check synthesis strategy and other timing related variables and then modify accordingly
SYNTH_STRATEGY of delay 0 will mean that the tool will focus more on optimizing the delay
index can changed to be 0, 1, 2, or 3 where 3 is the most optimized for timing at the cost of area.
SYNTH_BUFFERING of 1 will ensure that the buffer will be used on high fanout cells to reduce wire delay.
SYNTH_SIZING of 1 will enable cell sizing where cell will be upsized or downsized as needed to meet timing.
SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength.

After this, run synthesis again. It is will be seen that area is increased but there is no negative slack
tns = 0
wns = 0
Chip area for module picorv32a = 209181.872

Subsequently, we may run floorplan and placement
NOTE : If any error comes related to macro placement, temporary solution is to comment basic_macro_placement inside the file OpenLane/scripts/tcl_commands/floorplan.tcl (this is okay since we are not adding any macro to the design).

We can also execute the following commands:-

init_floorplan

place_io

global_placement_or

detailed_placement

tap_decap_or

detailed_placement

After successful run, runs/[date]/results/placement/picorv32a.placement.def will be created:-

After this, search for instance of cell sky130_vsdinv inside the DEF file after the placement stage through this command cat picorv32a.placement.def | grep sky130_vsdinv

Then, select a single sky130_vsdinv cell instance from the list dumped by grep (e.g. 41096). On tkcon, command select cell 41096 may be run. Press ctrl+z to zoom into that cell. As shown below, our customized inverter cell sky130_myinverter is sucessfully placed. Use the expand command on tkcon to show the footprint of the cell and notice how the power and ground of sky130_vsdinv overlaps the power and ground pins of its adjacent cells -:

Timing Analysis With Ideal Clocks Using Open STA
Setup Timing Analysis And Introduction to Flip-Flop Setup Time
Consider an ideal clock [i.e. the clock tree is not built yet] where perform timing analysis to understand the parameters [later the same can be done using real clocks]. Specifications are as mentioned in the programme [Clock frequncy (F) is 1GHz and clock period (T) is 1ns].

The setup timing analysis equation is = Θ < T - S

Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop

T = Time period, also called the required time

S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.

Introduction to Clock Jitter and Uncertainty
CLK signals are sent to the device by the PLL (Phase Locked Loop). This clock source is expected to send clock signal at 0, T, 2T etc. However, even these clock sources might or might not be able to provide a clock exactly at Tns because of its own in-built variation known as jitter. Jitter can be thought of as short-term fluctuations in the timing of signal transitions, which can result in deviations from the expected clock or data timing.

Hence, we see that a more realistic equation for setup time is = Θ < T - S - SU

SU = Setup uncertainty due to jitter which is temporary variation of clock period, which is due to non-idealities of PLL.

single corner - which only uses the LIB_TYPICAL library which is the one used in pre-layout(pos-synthesis) STA
multi corner - multicorner which uses LIB_SLOWEST(setup analysis, high temp low voltage),LIB_FASTEST(hold analysis, low temp high voltage), and LIB_TYPICAL libraries
In the location *Desktop/Work/tools/openlane_working_dir/openlane_ there is a file named pre_sta.conf on which we will be doing the prelayout analysis.

This is the SDC file on which analysis will be done is located at Desktop/Work/tools/openlane_working_dir/openlane/designs/picorv2a/src and is named *my_base.src_. It contains the following contents -:
The various commands to be run and their functions are -:

create_clock -it creates clock for the port with specified time period.

set_input_delay and set_output_delay - defines the arrival/exit time of an input/output signal relative to the input clock. [This is the delay of the signal coming from an external block and internal delay of the signal to be propagated to external ports] This adds a delay of Xns relative to clk to all signals going to input ports, and delay of Yns relative to CLK to all signals going to output ports.

set_max_fanout - specifies maximum fanout count for all output ports in the design.

set_driving_cell - models an external driver at the input port of the current design.

set_load sets a capacitive load to all output ports.

Execute sta pre_sta.conf and check timing.

Lab Steps to Optimize Synthesis to Reduce Setup Violations
To reduce negative slack, focus on large delays. Net with big fanout might cause delay increase. Use report_net -connections <net_name> to display connections. First thing we can do is to go back to OpenLane and reduce fanouts by setting ::env(SYNTH_MAX_FANOUT) 4 then run_synthesis again.

For reducing the negative slack obtained, we must focus on the large delays and try to reduce them. Increases in delays are majorly caused by nets with big fanouts, which result in high load cap which causes high delay. To display the connections of the nets, the commands report_net -connections <net_name> can be used. Now, looking upon this output, we realise that fanouts must be reduced. This can be done by going back to OpenLANE and reducing fanouts by typing ::env(SYNTH_MAX_FANOUT) 4. After this, we can run_synthesis again and obtain a expected value for slack.

Lab Steps to do Basic Timing ECO
However, if we want to reduce the negative slack even more, we can upsize the cells with high fanouts so that bigger drivers will be used. Now, since we cannot change load capacitance but can change the cell size, we can increase the size of the cell for a large driver to drive large cap loads leading to lesser delay. This is often done in iterations until we reach the desired slack in a process known as Timing ECO [Engineering Change Order]

Clock Tree Synthesis TritonCTS and Signal Integrity
Clock Tree Routing and Buffering Using H-Tree Algorithm

Take into account the CLK port going to the various flip-flops in the above picture. It's purpose is to connect the port to the CLK pins of the many flip-flops based on the connectivity information given. In the above picture, we have blindly made connections and hence t2 is greater than t1. Skew is the difference between t2 and t1. CLK skew, as explained earlier, refers to the variation in arrival times of the clock signal at different points within a digital system. More simply, CLK skew is essentially the difference in propogation delay of the CLK signal as it moves along various paths. It can occur due to -:

differences in wire lengths
variations in signal routing paths
variations in buffer delays
other physical and environmental factors
Due to this, some parts of the system can recieve CLK signals earlier than or after the rest of the system. Minimizing clock skew is essential as to -:

ensure proper synchronization of signals
reliable operation of the digital circuit.
NOTE : Ideally, the skew should be zero.

Now, since the skew is not minimum, or even close to minimum in the above picture, we can call this tree a bad tree. To optimize this scenario, we can use H-Tree routing. It analyses the clock route by calculating the distance from the source to all the endpoints and deciding on a midpoint to start building tree. Subsequently, it finds another midpoint and creates divergents in the wire. Eventually, after repeating this process, the CLK reaches all the flip-flops at almost the same time, while splitting up at various midpoints.

It is expected that the input CLK signal is exactly reproduced at the output. However, this does not happen in H-Tree routing due to inherent resistance and capacitance in physical wires, which may cause the signal to experience distortion. We can solve this problem by inserting buffers/repeaters along the path for signal integrity. In the past, we have also talked about another kind of repeaters, used in data paths. The key differences between repeaters used in clock and data paths respectively lie in their rise and fall times. Clock buffers have same rise and fall times, to ensure uniform signal propagation throughout the clock distribution network. Contrastingly, data buffers exhibit varying rise and fall times, which may differ based on the characteristics of the data being transmitted and the components involved in processing it.

Crosswalk and Clock Net Shielding
Clock nets are critical nets in the design because clock tree is built is such a fashion that the skew is zero. There is a phenomenon called crosstalk where a signal transmitted on one channel interacts with or interferes with signals on adjacent channels leading to distortion, noise, timing errors etc, which can cause the clock tree structure will be deteriorated. To solve this, all the clock nets are shielded [protected]. If there is a wire adjacent to such shields, then there exists a huge coupling capacitance causing two issues - (i) glitch and (ii) delta delay.

Whenever there is a switching activity happening in the aggressor, then the coupling capacitance is so strong that it directly affects the net close to it named the victim net, which is without any shielding. This causes a dip in the voltage, resulting in glitch. Due to a glitch, there can be incorrect data in memory, which can cause inaccurate functionality. To solve this, we do shielding which protects the victim nets by breaking the coupling capacitance between the aggressor and the victim. These shielding nets are either in the form of Vdd or Vss. The shields do not switch, and hence the victim will not switch as well.

Lab Steps to run CTS using TritonCTS
After completion of Timing ECO, we see that the timing currently is still above 1. To reduce it, we can go through the results and switch the cells causing a lot of slack to buffers. After this the negative slack is reduced to below 1.

After this, for OpenLANE to use the modified netlist, we can type write_verilog [filename] and then run floorplan and placement.

After running CTS, we can see that the clock buffers get added -:

Lab Steps to Verify CTS Runs
After the previous step, OpenLANE will take the procedures from /Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands and eventually invoke OpenROAD to run the tool.

For example, the command run_cts is found in the location /OpenLane/scripts/tcl_commands/cts.tcl. This tcl process will invoke OpenROAD which will call the cts.tcl which contains the OpenROAD commands for TritonCTS.

The file contains many configuration variables for CTS like-:

CTS_CLK_BUFFER_LIST -> which has the list of clock buffers used in clock tree branches (sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8)
CTS_ROOT_BUFFER -> this is the clock buffer used for the root of the clock tree and is the biggest clock buffer to drive the clock tree of the whole chip (sky130_fd_sc_hd__clkbuf_16)
CTS_MAX_CAP -> it is a numerical value for the maximum capacitance of the output port of the root clock buffer
Timing Analysis With Real Clocks Using Open STA
Setup Timing Analysis Using Real Clocks
Now the clock tree is built and timing analysis is done on real clocks.

delta1 = launch flop clock network delay

delta2 = capture flop clock delay

Any design satisfying Slack [i.e. Data required time - Data arrival time] is ready to work in the given frequency. However, if this equation is violated, then slack will become negative which is not expected. We expect slack to be either zero or positive.

Hold Timing Analysis Using Real Clocks
Hold Time is delay [time] needed by the MUX2 model within a flip-flop to transfer certain data outside. [i.e. how long it holds the data]. It is the time period during which the launch flop must retain dat before it reaches the capture flop. Unlike setup analysis, which has two rising clock edges, hold analysis occurs on the same rising clock edge for both the launch and capture flops.

A hold violation occurs when the path is too fast, impacted by factors such as -:

combinational delay
clock buffer delays
hold time.
Notably, parameters such as time period and setup uncertainty hold no significance, as both launch and capture flops receive identical rising clock edges during hold analysis.

Lab Steps to Analyse Timing With Real Clocks Using OpenSTA
The aim is to analyse the clock tree explained previously. The steps for analysis of timings with real clocks are -:

Enter into OpenROAD using the openroad command.
NOTE: In OpenROAD, timing is done slightly differently, where in a db is created from lef and def files and subsequently used

Subsequently create the db file through this command -: *read_lef [location of merged.lef]

Then, we must read the def file from the CTS stage

Subsequently create the db through this command -: write_db pico_cts.db

Then, read the db, verilog files, library and sdc

After this, set the propogated clock to calculate the actual delay in the clock path through the command -: set_propogated_clock [all_clocks]

Subsequently check the timings -:

Lab Steps to Excecute OpenSTA With Right Timing Libraries and CTS Assignment
TritonCTS is build to optimise only based on one corner. However, the libraries included previously that also take part in timing analysis optimise based on both min and max corners, which is not accurate. Hence, we need to exit and re-enter OpenROAD and then check timing only for typical corner.

NOTE: When the CTS is built, skew values are tried to be met by inserting buffers from CTS_CLK_BUFFER_LIST, which can be modified based on requirements.

When TritonCTS builds the clock tree, it tries to use each buffer listed in $::env(CTS_CLK_BUFFER_LIST) (sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8) from smallest to largest until the target skew is met.

It stores target skew in $::env(CTS_TARGET_SKEW).

The STA result also shows that sky130_fd_sc_hd__clkbuf_1 is the most used buffer, and we can also change the $::env(CTS_CLK_BUFFER_LIST) to use other buffers and observe the effect on STA and area.

We can also use tcl lreplace command to modify $::env(CTS_CLK_BUFFER_LIST)

The $::env(CURRENT_DEF) used by CTS is the DEF file of the previously run CTS, but the DEF file we want for CTS is the placement's DEF file. So to implement this, change the $::env(CURRENT_DEF) to point to placement DEF file then run_cts.

Now, observe the resulting post-CTS STA compared to previous run since we modified the clock buffer. Only buf_2 clock buffer is used now compared to buf_1 used in previous run, and hte WNS is better now since we have used bigger clock buffers.

Routing and Design Check [DRC]
Introduction to Maze Routing and Lee's Algorithm
Routing is to find the best possible connection [route] between two elements [clocks, flip-flops etc]. There have been many routing algorithms created like Steiner Tree algorithm, Line Search algorithm etc. and one such algorithm we will go into detail is Maze Routing - Lee's Algorithm (Lee 1961).

Consider connecting points 1 and 2 in the above picture, where in 1 is the source and 2 the target. The need is to find the best possible [shortest] path to connect 1 and 2. This route will aim to have very little or none routes with zig-zags, and mostly the routes are L-shaped. From an algorithmic standpoint, the software needs to search and connect the two poi+nts. From a physical design point of view, it is a physical/wire that allows signals to travel.

Lee's Algorithm is popularly used in maze routing [type of problem where path from source to destination needs to found in grid similar to a maze]. This algorithm is especially effective for routing in grid or mesh-based structures, making it an excellent choice for integrated circuit design.

Steps:

Initialisation - In this step, a routing grid/matrix is created in the area to be routed. It categorises each cell into one of various states - (i) obstacle ; (ii) empty ; (iii) visited ; (iv) source ; (v) target.

Wave Expansion - The algorithm performs a wave expansion from the source [marked as 'S'] cell, spreading outwards in all directions. At each step, the algorithm examines neighboring cells (up, down, left, and right) and assigns them a value one greater than the minimum value of their neighboring cells (excluding obstacles and starts from 1). This process continues until the target [marked as T] cell is reached or until no more cells can be visited.

Lee's Algorithm Conclusion
[Continuation of Steps]

Backtracking and Path Reconstruction - Once the target is reached, the algorithm backtracks the path to the source by following the cell values. Through this there might be multiple paths obtained but the tool will choose one with less bends i.e. a shorter route along the algorithm to give us the shortest route.
The route should not be diagonal and must not overlap any blockage/obstruction such as macros or HIPs.
Design Rule Check
When we route, we do not just merely connect two points, we must also adhere to certain rules. For example, one rule mention how when constructing two wires there must be a minimum distance between them, they must have a minimum width and pitch etc. ence, DRC cleaning is done to ensure that the routes can be fabricated and printed in silicon properly.

Another critical issue is signal short, which causes functionality failure. This can be removed by moving the route to the next layer. This leads to more DRCs (via width, via spacing, higher metal layer must be wider than lower metal layer etc.).

Power Distribution Network and Routing
Lab Steps To Build Power Distribution Network
The lab steps to build a power distribution network are -:

Go to the openlane directory

Enter the command docker and then ./flow.tcl -interactive . To subsequently get the openlane package, type package require openlane 0.9.

Then prep the design using -: **prep -design picorv32a -tag [folder name of run where in cts had been done]

Then type echo $::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/[folder name of run where in cts had been done]/results/cts/picorv32a.cts.def

Then, type this to generate the PDN -: gen_pdn

Lab Steps From Power Straps To STD Cell Power
The power and ground rails have a pitch of 2.72µm and hence the custom inverter cell also has a height of 2.72µm. This is as otherwise power and ground rails will not be able to power the cell. We also see that looking at the LEF file runs/[date]/tmp/merged.lef, all cells are have height of 2.72µm and only their width differs.

It is shown below how standard cells are powered up :- power/ground pads -> power/ground ring-> power/ground straps -> power/ground rails

Basics of Global and Detail Routing and Configure TritonRoute
TritonRoute is the engine that is used for routing through the run_routing command.

In the VLSI flow, the routing stage is highly critical and can be executed using either open-source or commercial tools. This stage is divided into two phases:

Global Route / Fast Route:

This is accomplished using fast routing techniques where the area to be routed is partitioned into tiles or rectangles. Global routing establishes the initial framework for routing paths. Detail Route:

This phase involves meticulous tracking routing techniques to complete the routing process. Detailed routing fine-tunes and finalizes the paths to ensure proper connectivity and compliance with design constraints.

In VLSI, routing is extremely important and is either done through open-source/commericial tools. It has mainly two phases -:

Global Route - Also known as fast route, it uses fast routing techniques wherein we partition the area to routed into tiles/rectangles. It establishes the initial framework
Detailed Route - This process uses more meticulous routing techniques, and completes the process by fine-tuning and finalizing the paths to ensure connectivity and compliance with DRC.
TritonRoute Features
TritonRoute Feature 1 - Honors Pre-processed Route Guides
The preferred direction for M1 and M2 respectively is vertical and horizontal. Whenever, a non-preferred direction route is found by the tool, the tool divides the route into unit width in a process called splitting. The sections that fall into the preferred directions are subsequently combined. Sections parallel to the preferred direction are bridged with upper layer [in bridging]. Non-preferred routes are converted into preferred routing guides of M2.

TritonRoute Feature 2 & 3 - Inter-Guide Connectivity and Intra & Inter Layer Routing
The second feature is interguide connectivity:-

As mentioned earlier, the preferred direction of M1 is vertical - resulting in vertically oriented lines. Panels are the dashed lines with a routing guide for each panel. Intra layer panel routing is when routing occurs withing even-index panels. Initially, routing takes place simultaneously in all even-index panels, which is followed by routing in odd-index panels. This routing remains confined within a particular layer and then Routing progresses from lower to upper layers, ensuring the orderly flow of routing operations.

The aim of MILP (Mixed Integer Linear Programming) is to find the optimal solution to connect two access point clusters.

In the below algorithm, cost is found for each access point. Then, a minimum spanning tree between access points and cost is made. So, the algorithm says that the minimal and most optimal point is needed between two APCs.

Routing Topology Algorithm and Final Files List Post Route
After running the run_routing command, routing [both global and detail] is completed. Routing Strategy was set to 0, which meant that DRC violations must be brought down from a high value [25000 in this case] to 0. This takes multiple iterations [34] and nearly 20 minutes to half an hour.

After this, a def file will be formed in the location [runs/[date]/results/routing/picorv32.def], which has to opened in MAGIC.

Parasitic Extraction
NOTE: OpenLANE does not have any spef extraction tool, so we use a separate tool present in work/tools/ directory.

The steps for extraction are -:

Go to /home/vsduser/Desktop/work/tools/SPEF_EXTRACTOR, where in there are a list of files, one of which is a python file called main.py. This file helps in generation of the SPEF provided. There are also lef & def files in the folder.

NOTE: spef will be saved in the same location as def file. /home/vsduser/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_05-49/tmp/routing

The last stage will be to extract the GDSII file ready for fabrication run_magic

This uses Magic to stream the GDSII file runs/26-03_05-49/results/magic/picorv32a.gds. This GDSII file can then be read by Magic:

The last stage is to extract the GDSII file ready for fabrication through run_magic. This uses MAGIC to stream the GDSII file runs/26-03_05-49/results/magic/picorv32a.gds. The GDSII can now be ready by MAGIC.
