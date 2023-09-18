# Inception of Open Source EDA, Openlane and Skywater130

To run Openlane, we run a docker container that contains a custom image.
Navigate to the openlane directory and run:
```
docker
./flow.tcl -interactive
```
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/8fc763ed-68a8-4dfb-b221-234a1560fcc1)

We must first import the openlane 0.9 package. This is simple enough, and can be achieved by running ```package require openlane 0.9```
<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/6c7b6907-7360-4e8e-96e7-128e1096423d)

Taking a look at our OpenLane directory, we can see the following files: 
<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/a549045a-08e1-451f-8231-404900e72a65)

The config file contains specific configuration switches used by Openlane.
<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/d5ed00f1-0b20-4198-bc66-9a1809f9ef75)

## Design flow on picorv32a

To prep the design, we must first run ```prep -design <design_name> -tag <tag>```, which in this case is
```
prep -design picorv32a
```
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/664548e5-682e-4eb0-a462-1b3cdcb92abf)

Post-prep, our results are stored in a directory,
<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/c8e30e52-bc5b-4980-92a6-ef8a4ff3c5eb)

Now, we execute the ```run_synthesis``` command, to synthesize the design.
<br><br>
![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/88ea3180-596b-4585-8aca-ae092414cf21)


![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/b2ba4726-212c-42c1-8853-cff6596c522f)

![image](https://github.com/Advaith-RN/pes_PhysicalDesignExploration/assets/77977360/015564d3-d61f-479e-a222-1b41005abafc)

From the above, the flop ratio can be calculated via ```No. of flops/No. of cells = 1613/14876 = 0.108```.
This comes out to 10.8%
