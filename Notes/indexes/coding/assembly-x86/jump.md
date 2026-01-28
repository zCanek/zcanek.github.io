*A jump instruction can cause the execution to switch to a completely new position in the program.* These jump destinations are generally indicated in assembly code by a *label*

Consider the following assembly-code sequence:
![[Pasted image 20250821143128.png]]
The instruction `jmp  .L1` will cause the program to skip over the `movq` instruction and instead resume execution with the `popq` instruction. In generating the object-code file, the assembler determines the addresses of all labeled instructions and encodes the jump targets (the addresses of the destination instructions) as part of the jump instructions.
![[Pasted image 20250821143052.png|600]]
The `jmp` instruction jumps *unconditionally*. It can be either a direct jump, where the jump target is encoded as part of the instruction, or an indirect jump, where the jump target is read from a [[register]] or a memory location. 
- Direct jumps are written in assembly code by giving a label as the jump target, for example, the label .L1 in the code shown. 
- Indirect jumps are written using `*` followed by an [[Operands - as|operand specifier]]. 

As examples, the instruction `jmp *%rax` uses the value in register `%rax` as the jump target, and the instruction `jmp *(%rax)` reads the jump target from memory, using the value in `%rax` as the read address. 

The remaining jump instructions in the table are *conditional*—they either jump or continue executing at the next instruction in the code sequence, depending on some combination of the [[3.6.1 Condition Codes|condition codes]]. The names of these instructions and the conditions under which they jump match those of the [[Indexes/Coding/Assembly x86/SET|SET]] instructions. As with the `SET` instructions, some of the underlying machine instructions have multiple names. Conditional jumps can only be direct.
# Encoding
![[3.6.4 Jump instruction Encodings]]
