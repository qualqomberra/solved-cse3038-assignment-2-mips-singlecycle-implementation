Download Link: https://assignmentchef.com/product/solved-cse3038-assignment-2-mips-singlecycle-implementation
<br>
In this project, you are required to extend the MIPS single-cycle implementation by implementing additional instructions. You will use ModelSim simulator [1] to develop and test your code. You are required to implement your assigned <strong>6 instructions</strong> (selected from following 28 instructions). <em>Note that your set of <strong>6 instructions</strong> will be emailed to you or your group member. </em>

<strong>R-format (11): balrn, balrz, sll, srl, sllv, srlv, jmadd, jmor, jmsub,</strong> <strong>jalr, jr</strong>

<strong>I-format (13):</strong> <strong>xori , andi, ori, bneal, bgez, bgezal, bgtz, blez, bltzal, bltz, jrs, jrsal,</strong> <strong>jpc</strong>

<strong>J-format (4): baln, balz, balv, jal</strong>

<strong> </strong>

You must design the revised single-cycle datapath and revised control units which make a processor that executes your instructions as well as the instructions implemented already in the design. After designing the new enhanced processor, you will implement it in Verilog HDL.

<strong><u>Specifications of New Instructions</u></strong>

<u>Instr</u>       <u>Type</u> <u>Code</u>                 <u>Syntax</u>                         <u>Meaning</u>







<ol>

 <li>xori I-type  opcode=14       xori $rt, $rs, Label     Put the logical XOR of register $rs and the zero-                                                                                    extended immediate into register $rt.</li>

 <li>andi I-type opcode=12        andi $rt, $rs, Label     Put the logical AND of register $rs and the zero-                                                                                    extended immediate into register $rt.</li>

 <li>ori I-type opcode=13         ori $rt, $rs, Label        Put the logical OR of register $rs and the zero- extended immediate into register $rt.</li>

 <li>bneal I-type opcode=45         bneal $rs, $rt, Target  if R[rs] != R[rt], branches to PC-relative address   (formed as beq &amp; bne do), link address is stored  in register 31</li>

 <li>bgez I-type opcode=39       bgez    $rs, Label        if  R[rs] &gt;= 0, branch to PC-relative address (formed</li>

</ol>

as beq &amp; bne do)

<ol start="6">

 <li>bgezal I-type opcode=35     bgezal $rs, Label        if  R[rs] &gt;= 0, branch to PC-relative address (formed</li>

</ol>

as beq &amp; bne do), link address is stored in register 31.

<ol start="7">

 <li>bgtz I-type opcode=38        bgtz     $rs, Label       if  R[rs] &gt; 0, branch to PC-relative address (formed</li>

</ol>

as beq &amp; bne do)

<ol start="8">

 <li>blez I-type opcode =6         blez     $rs, Label       if  R[rs] &lt;= 0, branch to PC-relative address (formed</li>

</ol>

as beq &amp; bne do)

<ol start="9">

 <li>bltzal I-type opcode=34 bltzal $rs, Label          if  R[rs] &lt; 0,  branch to to PC-relative address (formed                                                                                     as beq &amp; bne do), link address is stored in register 31.</li>

 <li>bltz I-type  opcode=1         bltz $rs, $rt, Target     if R[rs] &lt; 0, branches to PC-relative address (formed as</li>

</ol>

beq &amp; bne do)




<ol start="11">

 <li>balrz R-type funct=22 balrz $rs, $rd  if  Status [Z] = 1, branches to address found in register</li>

</ol>

$rs link address is stored in $rd (which defaults to 31)

<ol start="12">

 <li>balrn R-type funct=23 balrn $rs, $rd  if  Status [N] = 1, branches to address found in register</li>

</ol>

$rs link address is stored in $rd (which defaults to 31)

<table width="725">

 <tbody>

  <tr>

   <td width="216">13. jmadd R-type funct=32</td>

   <td width="509">jmadd $rs,$rt               jumps to address found in memory [$rs+$rt],                         link address     is stored in $31</td>

  </tr>

  <tr>

   <td width="216">14. jmor    R-type funct=37</td>

   <td width="509">jmor $rs,$rt                 jumps to address found in memory [$rs|$rt],                          link address     is stored in $31</td>

  </tr>

  <tr>

   <td width="216">15. jmsub R-type funct=34</td>

   <td width="509">jmsub $rs,$rt               jumps to address found in memory [$rs-$rt], link address</td>

  </tr>

  <tr>

   <td width="216"> </td>

   <td width="509">                                    is stored in $31</td>

  </tr>

  <tr>

   <td width="216">16. jalr      R-type func=9</td>

   <td width="509">jalr $rs, $rd      Unconditionally jump to the address found   in register $rs, link address is stored in $rd.</td>

  </tr>

  <tr>

   <td colspan="2" width="725">17. jr         R-type func=8             jr $rs                            Unconditionally jump to the address found in</td>

  </tr>

 </tbody>

</table>

register $rs

<ol start="18">

 <li>balv    J-type opcode=33        balv Target                 if Status [V] = 1, branches to pseudo-direct address</li>

</ol>

(formed as jal does), link address is stored in register 31 19. balz            J-type opcode=26        balz Target                  if Status [Z] = 1, branches to pseudo-direct address

(formed as jal does), link address is stored in register 31

<ol start="20">

 <li>baln J-type opcode=27        baln Target                  if Status [N] = 1, branches to pseudo-direct address</li>

</ol>

(formed as jal does), link address is stored in register 31

<ol start="21">

 <li>jal J-type opcode=3         jal Target         jump to pseudo-direct address (formed as j does),                                                                   link address is stored in register 31.</li>

 <li>jpc      I-type opcode=30        jpc Target                    jumps to PC-relative address (formed as beq and bne do)                                                                                    link address     is stored in $rt (which defaults to 31)</li>

 <li>jrs I-type opcode=18        jrs   $rs                        jumps to address found in memory where the memory                                                                                      address is written in register $rs.</li>

 <li>jrsal I-type opcode=19        jrsal   $rs                     jumps to address found in memory where the memory                                                                                   address is written in register $rs and link address is stored in memory (DataMemory[$rs]).</li>

 <li>sll R-type func=0             sll $rd, $rt, shamt       shift register $rt to left by shift amount (shamt) and store                                                                                      the result in register $rd.</li>

 <li>srl R-type func=2             srl $rd, $rt, shamt        shift register $rt to right by shift amount (shamt) and                                                                                      store the result in register $rd.</li>

 <li>sllv R-type func=4             sllv $rd, $rt, $rs        shift register $rt to left by the value in register $rs, and                                                                                              store the result in register $rd.</li>

 <li>srlv R-type func=6             srlv $rd, $rt, $rs         shift register $rt to right by the value in register $rs, and                                                                                            store the result in register $rd.</li>

</ol>




<strong><u>Status Register</u></strong>

Some of the conditional branches test the Z and N bits in the Status register. So the MIPS datapath will need to have a Status register, with the following 3 bits: Z (if the ALU result is zero) , N (if the ALU result is negative) or V (if the ALU result causes overflow). The Status register will be loaded with the ALU results each clock cycle.







<h1>Requirements</h1>

<strong> </strong>

<ul>

 <li>You will implement only 6 instructions. Therefore, you have to send a request email from canvas with introducing your group member.</li>

 <li>The deadline for the instruction request is <strong>05.2020 (Friday) 05:00pm</strong>. Please do not send me an email later than that date.</li>

 <li>Your design should support all your 6 instructions without corrupting other MIPS instructions.</li>

 <li>You can print out single-cycle datapath and start your design on a paper. Then you can continue with the Verilog implementation.</li>

 <li>A demo session will be scheduled in the following week of the submission. I request you to bring these design papers within you in demo sessions.</li>

 <li>You have to prepare a simulation of your instructions for the demo (including <em>register file</em>, <em>data memory</em> and <em>instruction memory</em>)!</li>

 <li>You have to do your projects in Verilog with using ModelSim simulator.</li>

 <li>You are allowed to use only the source codes in the attached. You cannot take any other implementations.</li>

 <li>You <strong>will not</strong> submit 6 different implementations. There should be only a single implementation supporting all your instructions.</li>

 <li>You are required to submit a minimum 2-pages report explaining implementation details of your project and commented code (via Canvas). Your report should include example runs for each instruction. Your implementation detail should be provided in the source code comment. All the files should be submitted as a single zip file. You should use your surnames as the name of the file: surname1_surname2_project2.zip</li>

</ul>