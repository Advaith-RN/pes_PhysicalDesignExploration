# Advanced Physical Design using OpenLANE/Sky130

## ASIC design requires 3 components:
- **RTL (Register-Transfer Level)** ```github.com, opencores.org, librecores.org```
  - RTL serves as a level of abstraction in digital design and electronics. It describes a system's behavior in terms of registers, logic gates, and the flow of data between them.

- **PDK (Process Design Kit)** ```Qflow, OpenRoad, OpenLane```
  - The Process Design Kit is a comprehensive collection of files, libraries, and documentation provided by semiconductor foundries. It serves as a guiding resource for designers in creating integrated circuits (ICs) compatible with a specific manufacturing process. PDKs encompass vital information on design rules, device models, and technology files essential for the design and simulation of ICs.

- **EDA (Electronic Design Automation)** ```Skywater```:
  - Electronic Design Automation tools are specialized software tools used for designing and testing electronic systems, including ICs and PCBs.

## RTL to GDS Flow
```
                  +------------------+
                  |    Synthesis     |     Convert HDL to logic gates
                  +------------------+
                           |
                           v
                  +------------------+
                  |   Floorplanning  |     Define chip layout and block placement
                  +------------------+
                           |
                           v
                  +------------------+
                  |   Powerplanning  |     Distribute power and minimize consumption
                  +------------------+
                           |
                           v
                  +------------------+
                  |    Placement     |     Position gates for timing and routability
                  +------------------+
                           |
                           v
                +----------------------+
                | Clock Tree Synthesis |   Create clock distribution network
                +----------------------+
                           |
                           v
                  +------------------+
                  |     Routing      |     Establish physical wire connections
                  +------------------+
                           |
                           v
                  +------------------+
                  |     Signoff      |     Verify design meets requirements
                  +------------------+
```

## OpenLane Design Flow

OpenLANE flow consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages.
<br><br>
<img src=https://user-images.githubusercontent.com/93475824/267214071-68749aa4-dd1a-401e-b7a3-0c74df581e8e.png>


1. Synthesis
    1. `yosys` - Performs RTL synthesis
    2. `abc` - Performs technology mapping
    3. `OpenSTA` - Pefroms static timing analysis on the resulting netlist to generate timing reports
2. Floorplan and PDN
    1. `init_fp` - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
    2. `ioplacer` - Places the macro input and output ports
    3. `pdn` - Generates the power distribution network
    4. `tapcell` - Inserts welltap and decap cells in the floorplan
3. Placement
    1. `RePLace` - Performs global placement
    2. `Resizer` - Performs optional optimizations on the design
    3. `OpenPhySyn` - Performs timing optimizations on the design
    4. `OpenDP` - Perfroms detailed placement to legalize the globally placed components
4. CTS
    1. `TritonCTS` - Synthesizes the clock distribution network (the clock tree)
5. Routing
    1. `FastRoute` - Performs global routing to generate a guide file for the detailed router
    2. `CU-GR` - Another option for performing global routing.
    3. `TritonRoute` - Performs detailed routing
    4. `SPEF-Extractor` - Performs SPEF extraction
6. GDSII Generation
    1. `Magic` - Streams out the final GDSII layout file from the routed def
    2. `Klayout` - Streams out the final GDSII layout file from the routed def as a back-up
7. Checks
    1. `Magic` - Performs DRC Checks & Antenna Checks
    2. `Klayout` - Performs DRC Checks
    3. `Netgen` - Performs LVS Checks
    4. `CVC` - Performs Circuit Validity Checks
  
Openlane runs in 2 modes:
- **Automated:** In this mode the entire flow is executed at once, for the design selected.
```
cd OpenLane
make mount
./ flow.tcl -design openlane/<DESIGN_NAME>  -tag <TAG>
```
- **Interactive:** In this mode the we run each step of the flow manually.
```
cd OpenLane
make mount
./flow.tcl -interactive 
prep -design <path_to_your_design_folder> -tag <tag> -overwrite //overwrite is optional
```
As mentioned, we can run each step of the flow manually.
```
run_synthesis
run_floorplan
run_placement
run_cts
run_routing
write_powered_verilog followed by set_netlist $::env(lvs_result_file_tag).powered.v
run_magic
run_magic_spice_export
run_magic_drc
run_lvs
run_antenna_check
```

### Day 1 - [Inception of opensource-EDA, Opennlane and Skywater130](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/edit/main/Day1.md)
