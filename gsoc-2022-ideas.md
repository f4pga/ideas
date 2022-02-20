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

_Duration_: 175 hours or 350 hours

_Mentor_: [@kgugala](https://github.com/kgugala)

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

_Duration_: 350 hours

_Mentor_: [@mkurc-ant](https://github.com/mkurc-ant)

## Improve the visual representation of the placement done by VTR

During the fasm2bels stage in f4pga-arch-defs, the [fasm](https://symbiflow.readthedocs.io/en/latest/fasm/docs/specification.html) information is transformed into a post-P&R design in form of a Verilog netlist and few TCL scripts that can be later on read by Vivado.
One of the scripts contains the information about which BELs should be instantiated and how they should be connected.
Currently, however there is no information about which BEL corresponds to which instance in the original netlist which makes the placement graph hard to read.
The idea is to implement a mechanism that will allow for better visual representation of the placement done by [VTR](https://verilogtorouting.org/) (Verilog to Routing) by highlighting the instances on various hierarchy levels with a set of colors.

### Expected Outcome

The result of this work should be a Vivado compatible script produced during the fasm2bels stage in the F4PGA flow that will perform the highlighting which can be later on viewed in the Vivado design viewer.

### Skills Required

* Familiar with the following tools: Yosys, Vivado
* Programming/scripting languages: TCL, C++

### Difficulty

Medium: This project requires some hands-on experience with the Yosys and Vivado tools as well some basic knowledge of the TCL scripting language. C++ is vital to enhance the existing tools in the F4PGA toolchain.

_Duration_: 175 hour or 350 hours

_Mentor_: [@acomodi](https://github.com/acomodi)

## Spartan6 bitstream documentation

Spartan6 is a popular part that is still used in many boards in the market today. There has already been some work in F4PGA with regard to this architecture. Namely, it’s possible to read the original bitstream and convert to a textual representation of its content in the form of what bits in which frame are active. F4PGA tools can also generate a bitstream from a textual representation.
The missing part falls into the scope of project X-Ray where the meaning of those bits found in the bitstream has to be determined.
The idea is to extend the existing set of project X-Ray infrastructure/tools/fuzzers to document the information which bits correspond to what features of the Spartan6 architecture

### Expected Outcome

As a result of this work some basic fuzzers required in X-Ray for a small Spartan6 FPGA (e.g. XC6SLX9) will be created.
One of them is the part fuzzer which produces the information about how many configuration columns and rows there are. Another is the tilegrid fuzzer which lists what tiles the FPGA is built on.

### Skills Required

* Familiar with the following tools: Vivado, ISE
* Programming/scripting languages: TCL, Python, C++
* HDL languages: Verilog

### Difficulty

Hard: This project requires some deeper understanding of FPGA architectures. Experience with the ISE tool and TCL scripting language is useful for this task. Python and C++ are vital to create or enhance existing tools used in X-Ray.

_Duration_: 350 hours

_Mentor_: [@tmichalak](https://github.com/tmichalak)

## Document XADC and `DNA_PORT` blocks for Xilinx Series 7

F4PGA’s FPGA flow depends on so-called [architecture definitions](https://github.com/SymbiFlow/f4pga-arch-defs), which are hardware descriptions of specific FPGAs that enable using specific configurable and hard blocks.
XADC is a dual 12-bit Analog-to-Digital converter block featured on the Xilinx Series 7 FPGAs, covered by [project X-Ray](https://github.com/SymbiFlow/prjxray).
The DNA port allows access to a dedicated shift register that can be loaded with some DNA data bits which can be factory programmed.
The task is to extend the existing fuzzer to document all XADC related configuration bits as well as create a new fuzzer for the DNA block so that they become available in the open source flow.

### Expected Outcome

As a result of this task 2 complete fuzzers shall be created:

* `XADC` fuzzer which will be the extended version of the existing fuzzer for XADC that will generate the list of XADC related features and the bits corresponding to them
* `DNA_PORT` fuzzer that will generate the list of DNA_PORT related features and corresponding bits

### Skills Required

* Scripting languages: TCL
* HDL languages: Verilog

### Difficulty

Easy: The task mostly requires getting familiar with the methodology used in other X-Ray fuzzers and reusing existing or coming up with your own approach to get the expected outcome.

_Duration_: 175 hours

_Mentor_: [@mkurc-ant](https://github.com/mkurc-ant)

## SystemVerilog preprocessor for Verible

SystemVerilog is the most popular HDL in ASIC design.
The open source ecosystem around SystemVerilog tooling is constantly growing.
One of the parts of the ecosystem is [Verible](https://github.com/chipsalliance/verible).
It’s a multi-purpose SystemVerilog parser that comes with various tools which can be used for linting, formatting or indexing SystemVerilog code.
It also comes with useful integrations like github actions (CI scripts) or LSP support.

Currently, verible mostly focuses on handling un-preprocessed files, which is fine for most of its use cases. However there are cases when the knowledge about the preprocessed code is absolutely necessary. One example would be macros that contain parts of code that is needed for the code outside of macros to make sense (e.g. the begin keyword when used inside a macro).

The goal of this project is to write a generic SystemVerilog parser library that can be used by Verible to preprocess SystemVerilog sources.

### Expected Outcome

A standalone library with a well defined API that can be used to preprocess SystemVerilog sources

### Skills Required

* Programming languages: C++, Bash, Python
* HDL languages: Verilog or SystemVerilog
* Operating system knowledge: Linux

### Difficulty

Hard: This project requires a good understanding of the language parsing process (lexing, parsing, preprocessing) but also a good understanding of linting and formatting concepts


_Duration_: 350 hours

_Mentor_: [@tgrochowik](https://github.com/tgorochowik)
