# pes_tff
# Contents
- [Full Subtractor](#full-subtractor)
- [Iverilog and yosys installation](#iverilog-and-yosys-installation)
- [GLS Process to verify Full Subtractor ](#gls-process-to-verify-full-subtractor)

## Full Subtractor
A full subtractor is a combinational circuit that performs subtraction of two bits, one is minuend and other is subtrahend, taking into account borrow of the previous adjacent lower minuend bit. 
This circuit has three inputs and two outputs. The three inputs A, B and Bin, denote the minuend, subtrahend, and previous borrow, respectively.
The two outputs, D and Bout represent the difference and output borrow, respectively. Although subtraction is usually achieved by adding the complement of subtrahend to the minuend, it is of academic interest to work out the Truth Table and logic realisation of a full subtractor; x is the minuend; y is the subtrahend; z is the input borrow; D is the difference; and B denotes the output borrow.
The corresponding maps for logic functions for outputs of the full subtractor namely difference and borrow.
### Block Diagram
![WhatsApp Image 2023-10-21 at 12 58 09 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/7f1584c5-0d00-463c-9674-04b8adeded26)



### Truth Table
![Screenshot 2023-10-21 125916](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/7de10509-f979-4025-97fd-ca7c05d76f4c)




## Iverilog and yosys Installation
- To install iverilog and gtkwave we type the following
```
- sudo apt-get update
- sudo apt-get install iverilog gtkwave
```

- To install yosys we type the following
```
- git clone https://github.com/YosysHQ/yosys.git
- sudo apt install make
- sudo apt-get install build-essential clang bison flex \
   libreadline-dev gawk tcl-dev libffi-dev git \
   graphviz xdot pkg-config python3 libboost-system-dev \
   libboost-python-dev libboost-filesystem-dev zlib1g-dev
```

- Now we type ```cd yosys``` and go into the yosys folder.
- Now we type
```
sudo make install
```
## GLS Process to Verify Full Subtractor

First we will look at the waveform simulation of the program 
![1](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/6b92736c-e23d-4d53-b753-5654621dd315)





- We first read the design and testbench file using the command
```
iverilog pes_full_subtractor.v pes_full_subtractor_tb.v
```
- Then we type ```./a.out``` to generate the .vcd file
- Now we type
```
gtkwave pes_full_subtractor_tb.vcd
```

The waveform is obtained as follows.'

![2](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/b974171f-2cb4-4bfb-85cc-fea0d6a1c0bd)



Now we will synthesize the design and generate the netlist file.
![3](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/c2ee0e7c-3b25-45b8-be02-a977a37a5761)



```
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_full_subtractor.v
synth -top pes_full_subtractor
```
![4](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/e81fda00-7404-4a8f-bee7-107436c85bc1)

![5](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/ce20dee7-3388-46ee-bea1-aa219fbffb90)


```
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
```
![6](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/4cd7f222-62c0-40a5-b493-43d0dfbc259e)


- We type ```show``` to display the design.

![7](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/a5c00860-a204-4b25-9d75-82a1c6c292b4)


- To generate the netlist file we must type the command
```
write_verilog -noattr pes_full_subtractor_netlist.v
```
![8](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/9f7c9c93-3636-41d1-81fd-e3cc0cad7c8a)


- Now using the netlist file, we verify the waveform once more

![9](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/719b8220-f29a-4595-b182-3d6bd3f0bdaa)


- To read the design and test bench file we must use the command
```
iverilog primitives.v sky130_fd_sc_hd.v pes_full_subtractor_netlist.v pes_full_subtractor_tb.v
```
- Now we type ```./a.out``` to generate the .vcd file.
- To see the waveform we type the command
```
gtkwave pes_full_subtractor_tb.vcd
```
- The following waveform is generated.
![10](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/50a8a53d-ff7e-452d-948c-3eec8fe89f5b)


- THe synthesis and simulation are matching
