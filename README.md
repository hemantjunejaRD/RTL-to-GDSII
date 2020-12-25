# Motive
Here, we will look in RTL to GDSII flow tool OpenLane and will try to run whole flow on RISCV core or on some part of RISCV. We will also look into multiple scripts which are running behind OpenLANE and how we can alter them to acheieve our objective. This will help us to get more detailed information about different stages of physical design flow like floorplan, placement, clock tree synthesis, global routing, detail routing, STA and in last physical verification.  

# Overview
OpenLANE: It is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII - this capability will be released in the coming weeks with completed SoC design examples that have been sent to SkyWater for fabrication.

![openlane flow 1](https://user-images.githubusercontent.com/31381446/103125490-3d81bb00-46b1-11eb-83f5-b9a40a56e91f.png)

As you can see in above figure, this explains alot about working of openlane. It consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages.

**1)  Synthesis** <br />
  a) yosys - Performs RTL synthesis. <br />
  b) abc - Performs technology mapping. <br />
  c) OpenSTA - Pefroms static timing analysis on the resulting netlist to generate timing reports. <br />

**2)  Floorplan and PDN** <br />
  a)  init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing). <br />
  b)  ioplacer - Places the macro input and output ports. <br />
  c)  pdn - Generates the power distribution network. <br />
  d)  tapcell - Inserts welltap and decap cells in the floorplan. <br />

**3)  Placement** <br />
  a)  RePLace - Performs global placement. <br />
  b)  Resizer - Performs optional optimizations on the design. <br />
  c)  OpenPhySyn - Performs timing optimizations on the design. <br />
  d)  OpenDP - Perfroms detailed placement to legalize the globally placed components. <br />

**4)  CTS** <br />
  a)  TritonCTS - Synthesizes the clock distribution network (the clock tree). <br />

**5)  Routing** <br />
  a)  FastRoute - Performs global routing to generate a guide file for the detailed router. <br />
  b)  TritonRoute - Performs detailed routing. <br />
  c)  SPEF-Extractor - Performs SPEF extraction. <br />

**6)  GDSII Generation** <br />
  a)  Magic - Streams out the final GDSII layout file from the routed def. <br />

**7)  Checks** <br />
  a)  Magic - Performs DRC Checks & Antenna Checks. <br />
  b)  Netgen - Performs LVS Checks. <br />

**OpenLANE integrated several key open source tools over the execution stages:** <br />

* RTL Synthesis, Technology Mapping, and Formal Verification : yosys + abc <br />
* Static Timing Analysis: OpenSTA <br />
* Floor Planning: init_fp, ioPlacer, pdn and tapcell <br />
* Placement: RePLace (Global), Resizer and OpenPhySyn (Optimizations), and OpenDP (Detailed) <br />
* Clock Tree Synthesis: TritonCTS <br />
* Fill Insertion: OpenDP/filler_placement <br />
* Routing: FastRoute (Global) and TritonRoute (Detailed) <br />
* SPEF Extraction: SPEF-Extractor <br />
* GDSII Streaming out: Magic <br />
* DRC Checks: Magic <br />
* LVS check: Netgen <br />
* Antenna Checks: Magic <br />
