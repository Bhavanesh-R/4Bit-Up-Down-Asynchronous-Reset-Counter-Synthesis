# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

PROGRAM:


COUNTER.V:

`timescale 1ns/1ns

module counter(clk,m,rst,count);

input clk,m,rst;

output reg [3:0] count;

always@(posedge clk or negedge rst)

begin

if (!rst)

count=0;

else if(m)

count=count+1;

else

count=count-1;

end

endmodule



COUNTER.TCL :

read_libs /cadence/install/FOUNDRY-01/digital/90nm/dig/lib/slow.lib

read_hdl counter.v

elaborate

read_sdc input_constraints.sdc 

syn_generic

report_area

syn_map

report_area

syn_opt

report_area 

report_area > counter_area.txt

report_power > counter.txt

write_hdl >counter.v

gui_show



COUNTER_TEST.V :

`timescale 1ns/1ns

module counter_test;

reg clk,rst,m;

wire [3:0] count;

initial

begin

clk=0;

rst=0;#5;

rst=1;

end

initial

begin

m=1;

#160 m=0;

end

counter dut(clk,m,rst,count);

always #5 clk=~clk;

initial $monitor("Time=%t rst=%b clk=%b count=%b" , $time,rst,clk,count);

initial

#320 $finish;

endmodule

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

◦ SDC (Synopsis Design Constraint) File (.sdc)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :
![Screenshot 2025-04-30 164416](https://github.com/user-attachments/assets/d867b288-5680-4fc1-b7e5-33b12b6ce11e)

#### Area report:
![WhatsApp Image 2025-05-07 at 10 23 49_6a6bc395](https://github.com/user-attachments/assets/4eea397c-a128-4f01-9c80-505e8960c71e)


#### Power Report:
![Screenshot (8)](https://github.com/user-attachments/assets/b05859a9-f430-407f-a163-d27ee306d8c7)

#### Timing Report: 
![Screenshot (7)](https://github.com/user-attachments/assets/458d4bfe-5b21-4301-94fa-72b3602cadbe)

#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





