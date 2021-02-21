# Tesseract Instruction Architecture - Rev 1.0.0


## Registers
### General Purpose Registers
### Special Registers

## Memory and Addressing Modes
### Declaring Static Data Regions
You can declare static data regions (analogous to global variables) using the directives DD can be used to declare one **Tryte** data locations. Declared locations can be labeled with names for later reference — this is similar to declaring variables by name, but abides by some lower level rules. For example, locations declared in sequence will be located in memory next to one another.

Example declarations:

		    DD 42		;Declare a tryte with no label, containing the value 42
    test:	DD 3xZC0	;Declare a tryte labeled 'test' containing the heptavintimal
    chr: 	DD 'c'		;Declare a tryte labeled 'chr' containing the ascii value of 'c'
    str: 	DD "Hello"	;Declare a succession of 5 trytes, labeled 'str', representing the ascii value of Hello

### Addressing Memory
### Size Directives

## Instructions
Machine instructions generally fall into several categories: data movement, arithmetic, logic, control-flow, and CPU-control. 

We use the following notation:
`<reg>` Any register
`<reg27>` Any 27-trit register
`<reg9>` Any 9-trit register
`<reg3>` Any 3-trit register
`<addr>` A memory address (e.g.,  [ax],  [var + 4], label)
`<const>` Any 3, 9, or 27-trit constant
`<const27>` Any 27-trit constant
`<const9>` Any 9-trit constant
`<const3>` Any 3-trit constant

### CPU Control
#### NOP
Does nothing, No OPeration

Syntax :

    NOP

#### HLT
Stops operation of the processor.

Syntax :

    HLT

### Data Movement
#### MOV
The mov instruction copies the data item referred to by its second operand into the location referred to by its first operand. While register-to-register moves are possible, direct memory-to-memory moves are not. In cases where memory transfers are desired, the source memory contents must first be loaded into a register, then can be stored to the destination memory address.

Syntax :
	
    MOV <reg>, <reg>
	MOV <reg>, <address>
	MOV <reg>, <constant>
	MOV <address>, <reg>
	MOV <address>, <constant>

#### PUSH
Pushes a value to the stack. The stack grows down and the current position is available in the stack pointer register (SP). This instruction will decrease the SP.

Syntax :

    PUSH <reg>
    PUSH <addr>
    PUSH <const>

#### POP
Pops a value from the stack to a register. This instruction will increase the SP.

Syntax :

    POP <REG>

### Control Flow
#### JMP - Unconditional jump
Let the instruction pointer do a unconditional jump to the defined address.

Syntax :

    JMP <addr>
    
#### Conditional Jump
Let the instruction pointer do a conditional jump to the defined address. See the table below for the available conditions.

| Instruction | Description |
|--|--|
| JC | jump if carry |
| JNC | jump if no carry |
| JUC | jump if unknow carry |
| JZ | jump if zero |
| JNZ | jump if not zero |
| JUZ | jump if unknow zero |
| JE | jump if equal to |
| JNE | jump if not equal to |
| JG | jump if greater than |
| JGE | jump if greater than or equal to |
| JL | jump if less than |
| JLE | jump if less than or equal to |

#### CMP
Compare the values of the two specified operands, setting the condition codes in the machine status word appropriately. This instruction is equivalent to the sub instruction, except the result of the subtraction is discarded instead of replacing the first operand.

Syntax :

    CMP <reg>, <reg>
	CMP <reg>, <addr>
	CMP <reg>, <const>

#### CALL
Call can be used to jump into a subroutine (function). Pushes the instruction address of the next instruction to the stack and jumps to the specified address.

Syntax :

    CALL <addr>

#### RET
Exits a subroutines by popping the return address previously pushed by the CALL instruction. Make sure the SP is balanced before calling RET otherwise the instruction pointer will have an ambiguous value.

Syntax :

    RET

### Logic Operation
#### AND, OR, XOR
These instructions perform the specified logical operation on their operands, placing the result in the first operand location.

Syntax:

    AND <reg>,<reg>  
	AND <reg>,<addr>   
	AND <reg>,<const>  
	AND <addr>,<reg> 
	AND <addr>,<const>  

	OR <reg>,<reg>  
	OR <reg>,<addr>  
	OR <reg>,<const>  
	OR <addr>,<reg>  
	OR <addr>,<const>  

	XOR <reg>,<reg>  
	XOR <reg>,<addr>  
	XOR <reg>,<const>  
	XOR <addr>,<reg> 
	XOR <addr>,<const>

#### CONS
#### ANY
#### EQUAL

#### STI, PTI, NTI
Logically negates the operand contents (that is, flips all trit values in the operand). STI stand for Standard Ternary Inverter, PTI for Positive Ternary Inverter and NTI for Negative Ternary Inverter.
In STI, 0 will remain 0, in PTI, 0 will be changed to + and in NTI, 0 will be changed to -

Syntax

    STI <reg>
	PTI <reg>
	NTI <reg>


#### SHL, SHR
These instructions shift the bits in their first operand's contents left and right, padding the resulting empty **Trit** positions with empty **Trit**. The shifted operand can be shifted up to 26 places. The number of trits to shift is specified by the second operand.

Syntax :

    SHL <reg>, <reg>
	SHL <reg>, <addr>
	SHL <reg>, <const>
	SHR <reg>, <reg>
	SHR <reg>, <addr>
	SHR <reg>, <const>

### Arithmetic Operation
#### INC, DEC
Increments or decrements a register by one. This operations will modify the carry and zero flag. SP can be used as operand with INC and DEC.

Syntax :

    INC reg
	DEC reg

#### ADD
The add instruction adds together its two operands, storing the result in its first operand. Note, whereas both operands may be registers, at most one operand may be a memory location.

Syntax:

    ADD <reg>,<reg>  
	ADD <reg>,<addr>   
	ADD <reg>,<const>  
	ADD <addr>,<reg> 
	ADD <addr>,<const>

#### SUB
The sub instruction stores in the value of its first operand the result of subtracting the value of its second operand from the value of its first operand. As with add

Syntax:

    SUB <reg>,<reg>  
	SUB <reg>,<addr>   
	SUB <reg>,<const>  
	SUB <addr>,<reg> 
	SUB <addr>,<const>
	
#### MUL
#### DIV
