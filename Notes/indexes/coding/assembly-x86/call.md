This instruction pushes an address $A$ onto the stack and sets the *PC* to the beginning of `Q`. The pushed address $A$ is referred to as the *return address* and is computed as the address of the instruction immediately following the `call` instruction. 
The counterpart instruction `ret` pops an address $A$ off the stack and sets the *PC* to $A$.
![[Pasted image 20250904134956.png|300]]
The call instruction has a target indicating the address of the instruction where the called procedure starts. Like [[JUMP]]s, a call can be either direct or indirect. The target of a direct call is given as a label, while the target of an indirect call is given by "`*`" followed by an [[3.4.1 Operand Specifiers|operand specifier]].
![[Pasted image 20250904135618.png]]
![[Pasted image 20250904135903.png|500]]
In this code we can see that the `call` instruction with address `0x400563` in `main` calls function `multstore`. This status is shown in Figure 3.26(a), with the indicated values for the [[program stack|stack]] pointer `%rsp` and the program counter `%rip`. The effect of the call is to *push the return address `0x400568` onto the stack and to jump to the first instruction in function multstore*, at address `0x0400540` (3.26(b)). 

The execution of function `multstore` continues until it hits the `ret` instruction at address `0x40054d`. This instruction pops the value `0x400568` from the stack and jumps to this address, resuming the execution of `main` just after the `call` instruction (3.26(c)). 
![[Pasted image 20250904141340.png|550]]
Each instruction is identified by labels `L1–L2` (in `leaf`), `T1–T4` (in `top`), and `M1–M2` in main. 
Part (b) of the figure shows a detailed trace of the code execution, in which main calls `top`(`100`), causing `top` to call `leaf`(`95`). Function `leaf` returns `97` to `top`, which then return `194` to `main`. 
	 Instruction `L1` of `leaf` sets `%rax` to 97, the value to be returned. Instruction `L2` then
returns. It pops `0x400054e` from the stack. In setting the PC to this popped value, control
transfers back to instruction `T3` of `top`. 
	The program has successfully completed the call to `leaf` and returned to `top`.
Instruction `T3` sets `%rax` to 194, the value to be returned from `top`. Instruction `T4` then returns. It pops `0x4000560` from the stack, thereby setting the PC to instruction `M2` of main. The program has successfully completed the call to `top` and returned to `main`. We see that the stack pointer has also been restored to `0x7fffffffe820`, the value it had before the call to `top`. 

We can see that this simple mechanism of pushing the return address onto the stack makes it possible for the function to later return to the proper point in the program.