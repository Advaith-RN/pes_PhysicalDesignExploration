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
