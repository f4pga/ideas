# F4PGA GSoC 2022 project ideas

1. [FPGA chips database visualizer improvements](#fpga-chips-database-improvements)
1. [DSP hard block integration in F4PGA](#dsp-hard-block-integration-in-f4pga)
1. [Improve the visual representation of the placement done by VTR](#improve-the-visual-representation-of-the-placement-done-by-vtr)
1. [Spartan6 bitstream documentation](#spartan6-bitstream-documentation)
1. [Document XADC and `DNA_PORT` blocks for Xilinx Series 7](#document-xadc-and-dna_port-blocks-for-xilinx-series-7)
1. [SystemVerilog preprocessor for Verible](#systemverilog-preprocessor-for-verible)
1. [F4PGA toolchain integration in mainline Edalize](#f4pga-toolchain-integration-in-mainline-edalize)
1. [FPGA Tool Performance Results Visualization](#fpga-tool-performance-results-visualization)
1. [Generalization of wrapper scripts for installed F4PGA toolchain and making them OS agnostic](#generalization-of-wrapper-scripts-for-installed-f4pga-toolchain-and-making-them-OS-agnostic)
1. [Symmetrical placement and routing APIs in OpenFASOC](#Symmetrical-placement-and-routing-APIs-in-OpenFASOC)

## FPGA chips database visualizer improvements

The [F4PGA database visualizer](https://github.com/chipsalliance/f4pga-database-visualizer) is an open source tool for visualizing the structure of FPGA chips covered by the F4PGA project. This is very useful in understanding the internal structure of specific FPGAs and reasoning about ways to best support them in the open source tools. Currently the tool supports visualizing the top level structure (tiles) of an FPGA chip as it is documented and modeled in open source toolchains.

### Task description

The main goal of the project is to extend the tool with the possibility of visualizing internal structures of the tile cells of an FPGA chip. This will require the following changes:

* Extending the frontend with functionality of rendering sub tiles within a tile, or with a functionality of “entering” a tile to see its details
* Extending the intermediate format used by the tool with functionalities required to handle multiple levels of the device
* (Nice to have) Adding a more detailed visualization of the tiles internals (Basic Logic Elements [like LUTs, DFFs], and their interconnections

## Expected outcomes

The work will result in improvements in the existing FPGA devices visualization software allowing it to show details of logic tiles of the visualized chip. As part of the work, the existing [visualization demo](https://chipsalliance.github.io/f4pga-database-visualizer/) will have to be extended to present the tile details.

### Required skills

Python, NodeJS, basics of HTML/CSS

### Difficulty

Medium: the project touches different aspects of the tool:

* Intermediate representation of the chip data
* Frontend and grid rendering

During the project it will be necessary to collaborate with the F4PGA development team to determine the best way of presenting the tiles details so that it is understandable to FPGA users.

_Duration_: 175 hours or 350 hours

_Mentor_: [@kgugala](https://github.com/kgugala)

### Further reading

* [Example chip visualization](https://chipsalliance.github.io/f4pga-database-visualizer/?dbfile=data%2Fprjxraydb%2Fartix7%2Fxc7a100t.json)
* [Visualizer repository](https://github.com/chipsalliance/f4pga-database-visualizer)

## DSP hard block integration in F4PGA

Modern FPGAs are equipped with multiple dedicated hard-blocks which can perform operations that would be slow and resource consuming if implemented as a part of a design in the fabric. An example of such a hard-block is a DSP (Digital Signal Processing) unit. DSP blocks can perform basic math operations like addition, multiplicaion and multiply-accumulate which are very common in signal processing pipelines.

The DSP hard block of the Xilinx 7-series FPGA devices (DSP48E) is currently documented in [Project X-Ray](https://github.com/SymbiFlow/prjxray), but it currently lacks support within the [F4PGA architecture definitions](https://github.com/SymbiFlow/f4pga-arch-defs). The aim of this project is to add support for DSP48E to the architecture definition so that it can be supported by the toolchain. This will enable designs using DSPs to be synthesized, placed and routed correctly.

This project will also aim at enabling the entire testing flow for designs using Xilinx 7-series DSP hard blocks to complete successfully.
The testing flow includes:

* Verilog-to-Bitstream using the F4PGA toolchain
* Fasm2bels to re-generate the original netlist from the bitstream output of F4PGA
* Proof-test through Vivado to verify the correctness of the netlist.

### Expected Outcome

This project will enable full support of DSP hard blocks within the F4PGA toolchain, which will translate to improving coverage for real world designs and more widespread adoption. 

### Skills Required

* Familiar with the following tools: Vivado, Yosys
* Programming/Scripting languages: Python, C++
* HDL: Verilog

### Difficulty

Hard: This project takes into consideration many different aspects of the toolchain, including the architecture generation infrastructure, synthesis and place and route. Moreover, the skill-set required to achieve the goal is very diverse.

### Further Reading

* [f4pga-arch-defs documentation](https://f4pga.readthedocs.io/projects/arch-defs/en/latest/)

_Duration_: 350 hours

_Mentor_: [@mkurc-ant](https://github.com/mkurc-ant)

## Improve the visual representation of the placement done by VTR

During the fasm2bels stage in f4pga-arch-defs, the [fasm](https://fasm.readthedocs.io/en/latest/) information is transformed into a post-P&R design in form of a Verilog netlist and few TCL scripts that can be later on read by Vivado.
One of the scripts contains the information about which BELs (Basic Logic Elements) should be instantiated and how they should be connected.
Currently, however there is no information about which BEL corresponds to which instance in the original netlist which makes the placement graph hard to read.
The idea is to implement a mechanism that will allow for better visual representation of the placement done by [VTR](https://verilogtorouting.org/) (Verilog to Routing) by highlighting the instances on various hierarchy levels with a set of colors.
Having a clear visualization of the post-placement instances and their mapping to the physical blocks of the FPGA would be a great benefit for the toolchain users, who can leverage this information to better understand which parts of the design may need optimization.

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

Spartan6 is a popular FPGA from AMD (formerly Xilinx) which is still used in many boards on the market today. For exactly this reason there is continuous interest in creating an open source toolchain for this architecture. There has already been some work in F4PGA with regard to this architecture. Namely, it’s possible to read the original bitstream and convert to a textual representation of its content in the form of what bits in which frame are active. F4PGA tools can also generate a bitstream from a textual representation.
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

Among other dedicated hard-blocks performing functionality that cannot be implemented directly using FPGA fabric Xilinx 7-series devices provide the `XADC` block and `DNA_PORT` block. The former is a generic dual 12-bit A/C converter capable not only of sampling external voltages but also reading the device's internal sensors like the temperature sensor. The latter block allows accessing unique device identification data a.k.a. "DNA".

F4PGA’s FPGA flow depends on so-called [architecture definitions](https://github.com/SymbiFlow/f4pga-arch-defs), which are hardware descriptions of specific FPGAs that enable using specific configurable and hard blocks. Documentation of Xilinx 7-series hard blocks is covered by [project X-Ray](https://github.com/SymbiFlow/prjxray).
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

## F4PGA toolchain integration in mainline Edalize

[Edalize](https://github.com/olofk/edalize) is a Python library to easily interface with many different EDA tools. It is heavily used in [FPGA-tool-perf](https://github.com/chipsalliance/fpga-tool-perf) to generate the setup for many different design builds.
Moreover, tools such as Edalize lower the barrier to approach the vast landscape of EDA tools, thus making it a great fit to enable the F4PGA toolchain support, greatly improving its usability.
Currently, [FPGA-tool-perf](https://github.com/chipsalliance/fpga-tool-perf) uses a forked version of Edalize, as it required some changes for the adoption of the F4PGA toolchain.
The idea is to move all the changes in upstream Edalize, to lessen the burden of maintaining the internal fork, with the advantage of integrating the most recent features of the upstream repository, such as the metrics extraction, which is currently handled externally in FPGA-tool-perf.

### Expected Outcome

FPGA-tool-perf uses the upstream Edalize library, with all the additional features to extract the relevant data, as well as a full integration of the F4PGA toolchain within the library.

### Skills Required

* Python
* FPGA tools usage

### Difficulty

Easy: This project is mostly aimed at improving and upstreaming an already existing development code.

_Duration_: 175 hours

_Mentor_: [@acomodi](https://github.com/acomodi)

## FPGA Tool Performance Results Visualization

[FPGA-tool-perf](https://github.com/chipsalliance/fpga-tool-perf) is a tool to run and profile different FPGA designs, supporting many toolchains, both open and closed source.
Its main focus is to gather information about different interesting metrics, such as run-time, frequency, area-utilization and more.
One of the great advantages of using such a tool is being able to verify and monitor the performance over time of all the different desings and toolchains, understand the overall status of the EDA ecosystem and identify what can be improved and optimized to achieve better results.
While the core of the tool has most of the functionalities supported, and performance data is correctly generated by CI, a proper visualization tool of said data is still lagging behind.
At the moment, the data is visualized on this [page](https://chipsalliance.github.io/fpga-tool-perf/) which is generated by the CI, but it requires some changes (both backend and visualization) to improve the performance of the code and improve the presentation layer of the collected results.

### Expected outcomes

The final product should be a user-friendly interface, preferably web-based, that is updated regularly and automatically with every CI run, which can display all the relevant metrics of the gathered data.

### Skills Required

* Web technologies (JavaScript, Web Frameworks)
* Python
* UX

### Difficulty

Medium: the project touches different aspects of the tool, from data generation (which might need to be adjusted accordingly during the project’s development) to data visualization.


_Duration_: 175 hours or 350 hours

_Mentor_: [@acomodi](https://github.com/acomodi)

## Generalization of wrapper scripts for installed F4PGA toolchain and making them OS agnostic

F4PGA aims to become a "GCC for FPGA" where a user can choose the target FPGA device merely by specifying different command line switches in the toolchain invocation command.
Currently in F4PGA there are wrapper scripts written in Bash that invoke the actual tools like Yosys, VPR and other helper Python scripts.
This takes away the need for the user to know the exact stages of the flow - which tools and in what order have to be executed to generate a bitstream given some specific HDL source code.

Unfortunately those scripts are currently specific to Xilinx 7-series devices.
Moreover adding support for a new architecture(s) and device(s) to F4PGA requires the modification of some hard-coded parameters that are part of the scripts.
It is also not possible currently to add another intermediate stage to the whole Verilog to bitstream flow. Being written in Bash also prevents from using the toolchain on OSs other than Linux (eg. on Windows).

### Expected Outcome

* As a result of this task the toolchain wrapper scripts will be re-written to Python and become common to all F4PGA architectures. The flow definition for each device will be read by them from a config file.
* The F4PGA toolchain runs on Linux, Windows and MacOS with the newly created Python wrappers. The flow on all three OSes is identical so that the end user is not influenced by the choice of OS.

### Skills Required

* Scripting languages: Bash, Python
* Operating system knowledge: Linux, Windows, MacOS

### Difficulty

Medium: The task does require more than just Bash to Python script conversion. It requires developing a unified way of describing the toolchain flow, as well as writing scripts which will use the description to broaden the scope to more platforms while making the flow more user friendly.


_Duration_: 175 hours or 350 hours

_Mentor_: [@mkurc-ant](https://github.com/mkurc-ant)


## Symmetrical placement and routing APIs in OpenFASOC

OpenFASOC is a framework used for automated IC design generation. It sits on top of tools such as OpenROAD primararly but recently is using tools such as gdsfactory and ALIGN. Currently, we have enabled a few py based functions that allow us to help with our analog layout requirements, and we are planning to create more general APIs that could be generalized to new analog designs.

### Expected Outcome

* As a result of this, it is expected to add new APIs that guides OpenROAD's placement/routing tools to enhance specific parts of the placement/routing steps (symmetry, guard banding, non default rules, symmetry, etc..)
* Call tools such as gdsfactory or ALIGN, to improve specific cells (aux cells) to improve the overall layout performance of the smaller circuits, routing or placement.

### Skills Required

* Scripting languages: Python, C++
* Operating system knowledge: Linux, Windows
* Nice to have: Circuit level and Physical Design basic understanding

### Difficulty

Medium: The task does require physical design understanding, python and C++ coding skills are needed


_Duration_: 350 hours

_Mentor_: [@msaligane](https://github.com/msaligane)
