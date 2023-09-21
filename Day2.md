# Chip Floorplanning Considerations

### 1. Define Width and height of core and die

- **Die** : Structure that consists of core which is a small semiconductor material on which the fundamental circuit is fabricated.
- **Core** : Structire that contains primary logic and functional components.

Whenever we come across the concepts of core and die, Utilisation factor plays an important role.
```
UTILISATION FACTOR = Area Occupied by the Netlist / Area of the core ----> (usually 50%-70%)
ASPECT RATIO = Height / Width -----> (1 = square, others = rectangle)
```

### 2. Define Location of Pre-Placed cells

**Pre-placed cells** : memories, clock gating cells, comparator, mux etc.

- The arrangement of these IPs on chip is called **Floorplanning**.
- These IPs have user-defined locations and are thus positioned on the chip before automated placement and routing, hence referred to as **pre-placed cells**.
- An automated **PnR (Placement and Routing)** tool is used to place the remaining logical cells in the design onto the chip.


### 3. De-coupling capacitors

In the realm of electronic circuits, it's a fundamental requirement to connect combinational blocks to ```Vdd``` and ```Vss``` for their proper functionality. However, in the case of expansive circuits laden with numerous resistors, an issue arises. The voltage in the logic components might not reach its intended levels due to voltage drops induced by the metallic wires and the presence of resistors along the electrical path. If, post-voltage drop, the voltage at the logic remains within the accepted noise margin, the circuit operates as expected. But what if it doesn't?

To tackle this predicament, we employ De-Coupling capacitors. These capacitors possess substantial capacitance and maintain a voltage level equal to that of the supply voltage. They are strategically placed in close proximity to the combinational logic elements. When the logic switches and experiences a surge in activity, these capacitors temporarily disengage the circuit from the primary power supply, stepping in as an alternative power source.

This ingenious solution ensures that local communication within the circuit remains robust and reliable. For global communication considerations, a comprehensive power planning approach is employed.

### 4. Power Planning

**Power Planning in Floorplanning:**
During the Floorplanning phase, effective power planning is crucial for mitigating noise in digital circuits resulting from voltage droop and ground bounce. This noise reduction involves addressing the coupling capacitance formed between interconnect wires and the substrate, which needs to be charged or discharged to represent either logic 1 or logic 0.

**Managing Charge Dumping and Ground Resistance:**
When a net undergoes a transition, the charge associated with coupling capacitors may be discharged to the ground. If an insufficient number of ground taps are available, charge accumulation can occur at the tap points. This accumulation can effectively turn the ground line into a large resistor, elevating the ground voltage and reducing our noise margin. To circumvent this issue, a robust Power Delivery Network (PDN) with numerous power strap taps is necessary. This design choice helps lower the resistance associated with the PDN, ensuring a more stable and noise-free operation.

### 5. Pin Placement

- Pin Placement plays a crucial role in the floorplanning process, serving to minimize buffering, enhance power efficiency, and optimize timing delays.

- Typically, we position input pins on the left side of the layout and output pins on the right side to streamline the signal flow.

- When it comes to primary inputs and outputs, pin sizes are usually kept small. In contrast, clock pins have larger sizes because they need to drive multiple cells, and reducing resistance is critical for clock signal integrity. This is where the adage holds true: larger area equals lower resistance.

- Placement Blockage is a strategic measure employed to ensure that no logic elements are placed in the region designated for pin placement. This practice helps maintain the integrity of the pin placement area.

# Floorplanning

Before running the floorplan, we make some changes to the config.tcl file.
<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/07d082c2-957b-422b-8610-4f3db658ef85)

Now, lets ```run_floorplan```<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/435be03c-ed1d-4a30-b401-4f1f441e746b)

The results will be shown in the runs folder.<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/866f9cab-b5c0-4238-9778-5383e666056f)

To view the layout, we use magic. First navigate to the correct directory.
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-09_14-17/results/floorplan

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/b4799c16-a226-49f3-bc1e-ca309753b7c3)
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/ce176cad-5cd4-4df0-b516-55b8b9ce839f)

Zooming into the design, we can see that the ports are equidistant.

# Library Binding and Placement
Move to the placement directory in results to view the placement.
To view it, we use:
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```

![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/75e1d628-5359-4d6d-8a61-b677af75a13e)

## Cell Design Flow

It is done in 3 parts:
- **Inputs:**
  - Process Design Kits (PDKs) containing DRC and LVS rules, SPICE models, libraries, and user-defined specifications.

- **Design Steps:**
  - Circuit Design
  - Layout Design (Euler Path and Stick Diagram)
  - Characterization

- **Outputs:**
  - Circuit Description Language (CDL)
  - GDSII
  - LEF
  - Extracted SPICE netlist (.cir)

**Standard Cell Library Characterization Flow:**

1. **Link Model File:**
   - Link the CMOS model file containing property definitions.

2. **Specify Process Corners:**
   - Define the specific process corners for the cell to be characterized.

3. **Specify Delay and Slew Thresholds:**
   - Specify the desired cell delay and slew thresholds as percentages.

4. **Specify Timing and Power Tables:**
   - Define the necessary timing and power tables for characterization.

5. **Read Parasitic Extracted Netlist:**
   - Read the parasitic extracted netlist associated with the cell.

6. **Apply Input or Stimulus:**
   - Apply the required input or stimulus to the cell for characterization.

7. **Provide Simulation Commands:**
   - Supply the essential simulation commands to perform the characterization.

This flow outlines the steps involved in characterizing cells within a Standard Cell Library using the open-source software GUNA, enabling the library's utilization by synthesis tools for optimizing circuit arrangements.

**Timing Characterization:**

- Slew_low_rise_thr = 20%
- Slew_high_rise_thr = 80%
- Slew_low_fall_thr = 20%
- Slew_high_fall_thr = 80%
- In_rise_thr = 50%
- In_fall_thr = 50%
- Out_rise_thr = 50%
- Out_fall_thr = 50%

- **Propagation Delay:**
  - Propagation delay = time(Out_fall_thr) - time(In_rise_thr)

- **Transition Time:**
  - On rise: time(Slew_high_rise_thr) - time(Slew_low_rise_thr)
  - On fall: time(Slew_high_fall_thr) - time(Slew_low_fall_thr)

[Back to Main](https://github.com/Advaith-RN/pes_PhysicalDesignExploration)


