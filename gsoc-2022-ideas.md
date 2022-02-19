# F4PGA GSoC 2022 project ideas

## FPGA chips database visualizer improvements

The database visualizer is an open source tool for visualizing the structure of FPGA chips covered by the F4PGA project. This is very useful in understanding the internal structure of specific FPGAs and reasoning about ways to best support them in the open source tools. Currently the tool supports visualizing the top level structure (tiles) of an FPGA chip as it is documented and modeled in open source toolchains.

### Task description

The main goal of the project is to extend the tool with the possibility of visualizing internal structures of the tile cells of an FPGA chip. This will require the following changes:

Extending the frontend with functionality of rendering sub tiles within a tile, or with a functionality of “entering” a tile to see its details
Extending the intermediate format used by the tool with functionalities required to handle multiple levels of the device
Nice to have feature will be a nice visualization of the tiles internals (Basic Logic Elements [like LUTs, DFFs], and their interconnections

## Expected outcomes

The result of this work will be an extension to existing FPGA devices visualization software allowing it to show details of logic tiles of the visualized chip. As part of the work, the existing [visualization demo](https://antmicro.github.io/symbiflow-database-visualizer/) will have to be extended to present the tile details.
### Required skills

Python, NodeJS

### Difficulty
Medium: the project touches different aspects of the tool:

* Intermediate representation of the chip data
* Frontend and grid rendering

During the project it will be necessary to determine the best way of presenting the tiles details so that it is understandable to FPGA users.

### Further reading

* [Example chip visualization](https://antmicro.github.io/symbiflow-database-visualizer/?dbfile=data%2Fprjxraydb%2Fartix7%2Fxc7a100t.json)
* [Visualizer repository](https://github.com/antmicro/symbiflow-database-visualizer)

## DSP hard block integration in F4PGA

The DSP (Digital Signal Processing) hard block is currently documented in [Project X-Ray](https://github.com/SymbiFlow/prjxray), and it currently lacks support within the [F4PGA architecture definitions](https://github.com/SymbiFlow/f4pga-arch-defs).
Support for a new primitive like the DSP hard block in the architecture means that the primitive can be synthesized, placed and routed correctly, and the whole testing flow of the DSP should complete successfully.
The testing flow includes the following:

* Verilog-to-Bitstream using the F4PGA toolchain
* Fasm2bels to re-generate the original netlist from the bitstream output of F4PGA
* Proof-test through Vivado to verify the correctness of the netlist.

### Expected Outcome

Full support of the DSP block within the toolchain

### Skills Required

* Familiar with the following tools: Vivado, Yosys
* Scripting language: Python, C++
* HDL: Verilog

### Difficulty

Challenging: This project takes into consideration many different aspects of the toolchain, including the architecture generation infrastructure, synthesis and place and route. Moreover, the skill-set required to achieve the goal is very diverse.

Further Reading

* [f4pga-arch-defs documentation](https://symbiflow.readthedocs.io/en/latest/symbiflow-arch-defs/docs/source/index.html)


