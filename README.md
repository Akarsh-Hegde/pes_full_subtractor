# pes_full_subtractor
# Contents
- [Full Subtractor](#full-subtractor)
- [Iverilog and yosys installation](#iverilog-and-yosys-installation)
- [GLS Process to verify Full Subtractor ](#gls-process-to-verify-full-subtractor)
- [Installation of ngspice magic and OpenLANE](#installation-of-ngspice-magic-and-openlane)
- [OpenLANE Flow](#openlane-flow)

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

![WhatsApp Image 2023-10-21 at 1 16 14 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/fc7ed296-2882-474e-a264-d5a96b312670)



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

  
## Installation of ngspice magic and OpenLANE

**ngspice**
- Download the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory
```
cd $HOME
sudo apt-get install libxaw7-dev
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
sudo make
sudo make install
```

**magic**
```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
sudo make
sudo make install
```

**OpenLANE**
```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
# After reboot
docker run hello-world (should show you the output under 'Example Output' in https://hub.docker.com/_/hello-world)

- To install the PDKs and Tools
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```

## OpenLANE Flow

- First we create a folder under the name of our design in the 'designs' folder.
- Do ```cd pes_ripple_counter```.
- Here we create a config.json file.
- We make a new directory called 'src'.
- Do ```cd src```
- We add the following files to this directory.
- All these files are found above in the 'pes_full_sutractor' folder.
- Now in the main 'Openlane' directory type ```mkdir pdks```.
- Copy paste the above file in it. Found in the verilog_model folder above.

- Type ```make mount``` in the main Openlane folder.

![WhatsApp Image 2023-11-04 at 11 10 24 AM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/46ee4821-341f-4206-b87d-07477edd9ec1)
  
- Then type ```./flow.tcl -interactive```.
- To prep the design type
```
prep -design pes_ripple_counter
```

### Synthesis
- Type
```
run_synthesis
```
![WhatsApp Image 2023-11-03 at 11 11 21 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/c8528300-6e82-4e8f-aa65-7e0fc285a5aa)
![WhatsApp Image 2023-11-03 at 11 07 58 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/0899fe9e-36f4-4bcb-9a79-258b8781c9e9)

**Physical Cells**

![WhatsApp Image 2023-11-03 at 11 10 37 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/db92fc15-9db4-428a-bc9a-77ed15ebeb45)

- Flop Ratio = 2/6 = 0.33

### Floorplan
- Now to run the floorplan we type
```
run_floorplan
```

![WhatsApp Image 2023-11-03 at 11 12 22 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/0273ad33-8cb1-4763-95af-f5b164f41392)


- To view the design we type
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def pes_tff.floorplan.def &
```

![WhatsApp Image 2023-11-03 at 10 45 58 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/a6228f88-8f5d-49e8-b869-723a03ff9c5e)
![WhatsApp Image 2023-11-03 at 10 44 50 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/65b5f60e-9a6f-46be-b017-cc705bd09c09)



### Placement
- Now to run the placement we type
```
run_placement
```
![WhatsApp Image 2023-11-03 at 11 13 01 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/72fd2400-19b1-4106-829e-0bce039585ac)

- To view the design we type
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def pes_tff.placement.def &
```
![WhatsApp Image 2023-11-03 at 10 48 42 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/f100ed2e-5ccc-4193-92e8-652b6051540c)
![WhatsApp Image 2023-11-03 at 10 48 24 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/4a09a901-0292-420e-9a4b-ae62a4da9a45)


### CTS(Clock Tree Synthesis)
- Now to run cts we type
```
run_cts
```
![WhatsApp Image 2023-11-03 at 11 14 06 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/ca725fdb-5139-4930-bd6a-6ae46c534fbe)


- The reports generated are given below


![WhatsApp Image 2023-11-03 at 11 21 53 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/008dfa68-879b-4d3b-bd18-f3c3161654d1)

![WhatsApp Image 2023-11-03 at 11 23 24 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/2ee6d3ff-3359-41c2-b072-bb41a3fcb3de)

**Power and Area Report**

<img width="576" alt="280473321-478ba7d3-8b5e-4bea-a215-bcef1263153b" src="https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/6b5de4e8-a478-462d-bbe3-3603ef087853">

<img width="576" alt="280473324-5b65a44a-1d1c-4f15-a102-ed2e68210eb5" src="https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/6b08f6e8-9fca-4d07-b4cb-9d8239a333e8">

<img width="715" alt="Screenshot 2023-11-04 at 8 01 41 PM" src="https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/1b59abcf-9a77-484f-af05-adb82d7aa193">

**Skew Report**

<img width="632" alt="Screenshot 2023-11-04 at 8 16 27 PM" src="https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/8ccf1d55-eeff-49ec-86fe-1755dd8444d9">


### Routing
- Now to run routing we type
```
run_routing
```
![WhatsApp Image 2023-11-03 at 11 14 49 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/6b7529b1-7cdb-45bc-930a-b931a3b10e02)


- To view the design we type
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def pes_tff.def &
```
![WhatsApp Image 2023-11-03 at 10 53 31 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/1ceb5651-b52b-428a-810d-14fb7d8066f7)
![WhatsApp Image 2023-11-03 at 10 52 51 PM](https://github.com/Akarsh-Hegde/pes_full_subtractor/assets/79705687/a6a14e55-b4a6-40ed-a157-0cf6c995409c)


**Statistics**
- Area = Area = 238 u^2
- Internal Power = 6.28e-06 W
- Switching Power = 1.05e-05 W
- Leakage Power =  1.47e-10 W
- Total Power = 1.67e-05 W (100 %)
