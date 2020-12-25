# Motive
Here, we will look in RTL to GDSII flow tool OpenLane and will try to implement whole flow on our design to get a clean layout. 

# Overview
OpenLANE: It is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII - this capability will be released in the coming weeks with completed SoC design examples that have been sent to SkyWater for fabrication.

![openlane flow 1](https://user-images.githubusercontent.com/31381446/103125490-3d81bb00-46b1-11eb-83f5-b9a40a56e91f.png)

As you can see in above figure, this explains alot about working of openlane. It consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages.

1)  **Synthesis**
  a) yosys - Performs RTL synthesis
  b) abc - Performs technology mapping
  c) OpenSTA - Pefroms static timing analysis on the resulting netlist to generate timing reports.

2)  **Floorplan and PDN**
  a)  init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
  b)  ioplacer - Places the macro input and output ports
  c)  pdn - Generates the power distribution network
  d)  tapcell - Inserts welltap and decap cells in the floorplan

3)  **Placement**
  a)  RePLace - Performs global placement
  b)  Resizer - Performs optional optimizations on the design
  c)  OpenPhySyn - Performs timing optimizations on the design
  d)  OpenDP - Perfroms detailed placement to legalize the globally placed components

4)  **CTS**
  a)  TritonCTS - Synthesizes the clock distribution network (the clock tree)

5)  **Routing**
  a)  FastRoute - Performs global routing to generate a guide file for the detailed router
  b)  TritonRoute - Performs detailed routing
  c)  SPEF-Extractor - Performs SPEF extraction

6)  **GDSII Generation**
  a)  Magic - Streams out the final GDSII layout file from the routed def

7)  **Checks**
  a)  Magic - Performs DRC Checks & Antenna Checks
  b)  Netgen - Performs LVS Checks

**OpenLANE integrated several key open source tools over the execution stages:**

* RTL Synthesis, Technology Mapping, and Formal Verification : yosys + abc
Static Timing Analysis: OpenSTA
Floor Planning: init_fp, ioPlacer, pdn and tapcell
Placement: RePLace (Global), Resizer and OpenPhySyn (Optimizations), and OpenDP (Detailed)
Clock Tree Synthesis: TritonCTS
Fill Insertion: OpenDP/filler_placement
Routing: FastRoute (Global) and TritonRoute (Detailed)
SPEF Extraction: SPEF-Extractor
GDSII Streaming out: Magic
DRC Checks: Magic
LVS check: Netgen
Antenna Checks: Magic
