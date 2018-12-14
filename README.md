## IRN's Vivado HLS Code

This repository contains the Xilinx Vivado HLS code for synthesizing IRN's packet processing logic, as a proof-of-concept for its implementation feasibility. It reproduces the results presented in Sec 6.2 of IRN's paper, titled "Revisiting Network Support for RDMA", that appeared in SIGCOMM'18. 

#### Contents of the Repository

There are two folders in the repository:

**irn_receiver**: This folder contains the code for the receiver side module at the responder, namely *receiveData*.

**irn_sender**: This folder contains the code for sender side modules at the requester, namely *txFree*, *receiveAck* and *timeout*.

Each folder contains scripts to run synthesis, RTL verification and exporting for each module. A sample trace for input to the respective modules and the output, generated via simulations, has also been provided for the verification. 

#### Usage

The code assumes availability of Xilinx Vivado HLS 2017.2 (or any other HLS version that supports the target device Kintex Ultrascale XCKU060). 

The code can also be used with the free Xilinx Vivado Webpack edition, by changing the target device specified in thje `run_hls*.tcl` files inside the *irn_receiver* and the *irn_sender* folders to one of those that are supported by the Webpack edition, and editing the following line to match the clock rate supported by the device. However, the synthesis result would then differ from what is reported in the paper. 

###### IRN Receiver

1. Go to *irn_receiver* folder. All path names in this section are specified relative to this folder.

2. Update the second line in file 'megarun.sh' to set the correct path to the licence file. 

3. Run 'megarun.sh'.

4. A new folder called *rdma_receiver*, that contains the synthesis results, is created within the *irn_receiver* directory. 

5. The synthesis report can be found in *rdma_receiver/solution-128/syn/report/receiveData_csynth.rpt*. It provides the performance and resource usage results that were used to generate the first row of Table 2, in Section 6.2 of the paper.  

6. A trace output, generated by the testbench, can be found at *rdma_receiver/solution-128/sim/wrapc/output.txt* and can be compared against *traces/recvOutput-40Gbps-incast.txt*. The latter is the output trace generated by the receiver nodes in the simulator used in Section 4 of the paper for the same input packet trace that was supplied to the Vivado HLS code. Generation of an identical output file by the HLS code indicates that it follows the same transport logic as the simulator code. More details about these traces can be found in *traces/description.txt*.   

###### IRN Sender

1. Go to *irn_sender* folder. All path names in this section are specified relative to this folder. 

2. Update the second line in file 'megarun.sh' to set the correct path to the licence file. 

3. Run 'megarun.sh'.

4. For each *$module* in *(txFree, receiveAck, timeout)*, new folders that contain the respective synthesis results are created within *irn_sender*. These folders are named *rdma_sender_$module* (i.e. *rdma_sender_txFree*, *rdma_sender_receiveAck*, and *rdma_sender_timeout* respectively).

5. For each *$module* in *(txFree, receiveAck, timeout)*, the synthesis report can be found in *rdma_sender_$module/solution-128/syn/report/$module_csynth.rpt*. They provide the performance and resource usage results that were used to generate the second, third and fourth rows of Table 2, in Section 6.2 of the paper.

6. For each *$module* in *(txFree, receiveAck, timeout)*, a trace output, generated by the testbench, can be found at *rdma_sender_$module/solution-128/sim/wrapc/output.txt* and can be compared against *traces/sendOutput-40Gbps-incast.txt*. The file *traces/sendOutput-40Gbps-incast.txt* needs to be created by concatenating *traces/sendOutput-40Gbps-incast-part1.txt* with *traces/sendOutput-40Gbps-incast-part2.txt*. It is the output trace generated by the sender nodes in the simulator used in Section 4 of the paper for the same input trace that was supplied to the Vivado HLS code. More details about these traces can be found in *traces/description.txt*. 
