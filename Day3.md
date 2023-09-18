# ngspice simulations for CMOS Inverter

## Fabricating a CMOS Inverter 

1. **Selecting the Substrate:**
   - Use a P-Type substrate with resistivity around 5-50 ohms, doping level of 10^15 cm^-3, and orientation (100).
   - Ensure that the substrate doping is lower than well doping (used for NMOS and PMOS fabrication).

2. **Create Active Regions:**
   - Grow a layer of SiO2 (~40nm) on the P-substrate.
   - Deposit approximately 80nm of Si3N4 on the SiO2 layer.
   - Deposit a 1um layer of photoresist for region definition.
   - Perform photolithography and etch out Si3N4 and SiO2 using a suitable solvent.
   - Place the structure in an oxidation furnace to grow field oxide (LOCOS - Local Oxidation of Silicon).
   - Etch out Si3N4 using hot phosphoric acid.

3. **N-Well and P-Well Formation:**
   - Apply photoresist and a mask to cover NMOS regions.
   - Expose to UV, wash, remove the mask, and apply boron (p-type) using Ion Implantation at 200Kev (for diffusion).
   - Repeat the process for the other half using phosphorus @400Kev (heavier) for the P-Well.
   - Subject the wafers to a high-temperature furnace to increase the well depth.

4. **Formation of Gate:**
   - Repeat the previous step at a lower energy with p-type implant (boron @60Kev) and n-type implant (Arsenic).
   - The SiO2 is damaged as dopants penetrate through it. Etch out the original SiO2 using dilute HF solution and regrow it for a high-quality oxide (~10 nm thin).
   - Apply N-type ion implants for low gate resistance.
   - Mask a small width of Nwell and Pwell above SiO2 and perform photolithography.
   - Gate formation is complete.

5. **Lightly Doped Drain Formation (LDD Formation):**
   - On the SiO2 surface corresponding to NWell, apply photoresist, mask it, and apply phosphorous to create an N-Implant on Pwell (N-).
   - Perform a similar process for the other side using boron for (P-) implant.
   - Protect LDD with a 0.1um thick SiO2 layer and form side wall spacers using plasma anisotropic etching.

6. **Source and Drain Formation:**
   - Mask the Nwell structure and deposit arsenic @75KeV for N+ implant on Pwell.
   - Use boron for P+ implant formation on Nwell.
   - Subject the wafer to a high-temperature furnace to achieve the required thickness of N+, P+, N-, and P- implants.

7. **Steps to Form Contacts and Interconnects:**
   - Etch thin SiO2 oxide in HF solution.
   - Deposit Titanium on the wafer surface using sputtering.
   - Heat the wafer at 600-700 degrees in an ambient N2 environment for 60 seconds to create low-resistance TiSi2 at the gate regions.
   - Form TiN for local communication.
   - Etch off TiN around gate structures of both MOS using RCA Cleaning.

8. **Higher Level Metal Formation:**
   - Deposit a thick layer (1um) of SiO2 doped with P/B (phosphoborosilicate glass) on the structure.
   - Use CMP (Chemical Metal Polishing) to make the added surface flat.
   - Create contact pins by making holes with contacts using Al, W, and TiN layer depositions.
   - Deposit a layer of Si3N4 to act as a dielectric for chip protection.

We also require some external design files, which are present in this repo.
```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
Copy the ```sky130A.tech``` and ```sky130_inv.mag``` file into a new directory called ```vsdstdcell``` in the ```openlane``` directory.
Now we can view it using magic.
```
magic -T sky130A.tech sky130_inv.mag
```
<br><br>

![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/b3c31864-a95c-45ed-b273-2c1645929005)


