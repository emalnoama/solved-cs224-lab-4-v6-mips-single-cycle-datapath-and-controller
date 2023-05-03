Download Link: https://assignmentchef.com/product/solved-cs224-lab-4-v6-mips-single-cycle-datapath-and-controller
<br>
<strong>Part 1 is Mandatory, Part 2 is Optional!</strong>




<strong>Purpose</strong>: In this lab you will use the digital design engineering tools (System Verilog HDL, Xilinx Vivado) to modify the single-cycle MIPS processor.  You will expand the instruction set of the “MIPS-lite” processor by adding new instructions to it. To do this, you must first determine the RTL expressions of the new instructions then modify the datapath and control unit of the MIPS.  To implement the new instructions will require you to modify some System Verilog modules in the HDL model of the processor.

To test and prove correctness, you will simulate the microarchitecture




<h1>Summary</h1>

<strong>Part 1</strong> (100 points): System Verilog model for Original10 + New instructions. You will submit a PDF report plus written code pieces for this part. There will be separate assignments for each Section in Unilica to submit your PDF and your code.

<strong>Part 2</strong> (Optional): Simulation of the MIPS-lite processor and Implementation and Testing of new instructions. You will NOT submit your code for this part.




<strong>If we suspect that there is cheating, we will send the work with the names of the students to the university disciplinary committee. </strong>




<strong><u>[REMINDER!]</u></strong> In this lab and the next one, we assume SystemVerilog knowledge, as well as the ability to use tools such as Xilinx Vivado, since you all took CS223 – Digital Design. If you are not familiar with these, please get used to them as soon as possible.







<h1>Part 1. (100 points)</h1>




As part of the Part 1 you will prepare a report that contains the following 7 items, one per page. Also you will submit your codes along with your PDF.




<ol>

 <li>Cover page, with university name, department name, and course name and number at the top, “Design Report”, Lab # (e.g. 4), Section #, and your name and ID# in the middle, and the date of your lab at the bottom.</li>

</ol>




<ol>

 <li><strong>[15 points] </strong>Determine the assembly language equivalent of the machine codes given in the imem module in the “Complete MIPS model.txt” file posted on Unilica for this lab. In the given System Verilog module for imem, the hex values are the MIPS machine language instructions for a small test program. Dis-assemble these codes into the equivalent assembly language instructions and give a 3-column table for the program, with one line per instruction, containing its location, machine instruction (in hex) and its assembly language equivalent. [Note: you may dis-assembly by hand or use a program tool like the one in Unilica.]</li>

</ol>




<ol>

 <li><strong>[15 points] </strong>Register Transfer Level (RTL) expressions for each of the <u>new instructions</u> that you are adding (see list below for your section), including the fetch and the updating of the PC.</li>

 <li><strong>[20 points] </strong>Make any additions or changes to the datapath which are needed in order to make the RTLs for the instructions possible. The base datapath should be in black, with changes marked in red and other colors (one color per new instruction).</li>

 <li><strong>[25 points] </strong>Make a new row in the main control table for each <u>new instruction</u> being added, and if necessary, add new columns for any new control signals that are needed (input or output). Be sure to completely fill in the table—all values must be specified. If any changes are needed in the ALU decoder table, give this table in its new form (with new rows, columns, etc). The base table should be in black, with changes marked in red and other colors.  {Note: if you need new ALUOp bits to encode new values, you should also give a new version of Table 7.1, showing the new encodings}</li>

 <li><strong>[25 points] </strong>Write a test program in MIPS assembly language, that will show whether the new instructions are working or not, and that will confirm that all existing old instructions still continue to work. Don’t use any pseudo-instructions; use only real MIPS instructions that will be recognized by the new control unit.</li>

</ol>




<h2>Table of instructions to implement– by section</h2>

<table width="325">

 <tbody>

  <tr>

   <td width="62">Section</td>

   <td colspan="3" width="263">MIPS instructions</td>

  </tr>

  <tr>

   <td width="62">Sec 1</td>

   <td colspan="3" width="263">Base: Original10 New: “ble”, “subi”</td>

  </tr>

  <tr>

   <td width="62">Sec 2</td>

   <td colspan="3" width="263">Base: Original10New: “nop”, “sw+”</td>

  </tr>

  <tr>

   <td width="62">Sec 3</td>

   <td colspan="3" width="263">Base: Original10New: “bge”, “swapRM”</td>

  </tr>

  <tr>

   <td rowspan="2" width="62">Sec 4</td>

   <td colspan="3" width="263">Base: Original10</td>

  </tr>

  <tr>

   <td width="86">New: “jm”,</td>

   <td width="88">ll/sc OR “bgt”</td>

   <td width="89"></td>

  </tr>

  <tr>

   <td width="62">Sec 5</td>

   <td colspan="3" width="263">Base: Original10 New: “jalm”, lui</td>

  </tr>

  <tr>

   <td width="62">Sec 6</td>

   <td colspan="3" width="263">Base: Original10 New: jalr, “push”</td>

  </tr>

 </tbody>

</table>




The Original10 instructions in “MIPS-lite” are add, sub, and, or, slt, lw, sw, beq, addi and j.




Instructions in quotes (e.g. ”push”) are not defined in the MIPS instruction set. They don’t exist in any MIPS documentation; they are completely new to MIPS.  You will create them, according to the definitions below, then implement them.




bge, ble, bgt: these I-type instructions do what you would expect—branch to the target address, if the condition is met. Otherwise, the branch is not taken. Example: bge $t2, $t7, TopLoop




jalm: : this I-type instruction is a jump, to the address stored in the memory location indicated in the standard way.  But it also puts the return address into the register specified.  Example: jalm $t5, 40($s3)




jm: this I-type instruction is a jump, to the address stored in the memory location indicated in the standard way. Example: jm 40($s3)




nop: this I-type instruction does nothing, changes no values, takes one clock cycle. Except for the opcode (which you need to assign a code to), the I-type instruction field values are irrelevant. Example: nop

{Note: an R-type nop exists in real MIPS, it is sll $0, $0, 0.  But the nop here is different, so it is “nop” !}




push: this I-type instruction does what you would expect—push a register value onto stack. Example: push $a3 {Note: the assembler automatically puts 29 in the rs field, and 0 into the immediate field, of the machine code instruction.}




subi: this I-type instruction subtracts, using a sign-extended immediate value. subi $t2, $t7, 4




swapRM: this I-type instruction exchanges the data in two locations: the exchange is between a register value and a memory value (addressed in the standard way).  Example:  swapRM $v0, 1004($sp)




sw+: this I-type instruction does the normal store, as expected, plus an increment (by 4, since it is a word transfer) of the base address in RF[rs]. {Note: these kind of auto-increment instructions are useful when moving through an array of data words.}

<strong> </strong>

<h2>Part 2:  Simulation and Implementation (Optional)</h2>

<ol>

 <li>Complete the System Verilog model of single-cycle MIPS by designing a 32-bit ALU module (one is partly specified already in “Complete MIPS model.txt”) and save this ALU module by itself in a new file with a meaningful name. Make this file the basis of a new Xilinx Vivado project. In simulation, check its syntax, and then simulate this ALU, using a testbench that you will write.  When you are sure that the 32-bit ALU is working correctly in simulation, you can now use it in the MIPS-lite datapath.  When you have integrated your working 32-bit ALU into the “Complete MIPS model.txt” file, you are ready to simulate the MIPS-lite single-cycle processor in Xilinx Vivado.</li>

 <li>Make a New Project, giving it a meaningful name, for your single-cycle MIPS-lite. Do Add Source for the System Verilog modules given in “Complete MIPS model.txt” (modified with your working ALU), and Save everything (you don’t have to use Complete MIPS model.txt, you can write your code if you find it more convenient). Make the necessary changes in order to support newly added instructions.</li>

 <li>Study the small test program loaded into instruction memory (in the imem module) that you disassembled in part b) of your Preliminary Design Report. What is the program attempting to do? Extend the imem module so that it will also include newly added instructions as well.</li>

 <li>Now make a System Verilog testbench file and using Xilinx Vivado, simulate your MIPS-lite processor executing the test program. Study the results given in the simulation window. Find each instruction, and understand its values. Why is writedata undefined for some of the early instructions in the program?</li>

 <li>Now modify the simulation, in order to show more information. Make changes to the System Verilog modules as needed so that the 32-bit values of PC and the Instruction are made to be outputs of the top-level module. Then modify the testbench file, so that they are displayed in the simulation.</li>

</ol>

<strong> </strong>


