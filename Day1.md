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
![Uploading image.png…]()
