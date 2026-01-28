![[Pasted image 20250819173309.png]]
In binary operations the second [[Operands - as|operand]]  is used as both a source and a destination. Note that the source operand is given first and the destination second. This looks peculiar for non-commutative operations. For example, the instruction `subq %rax,%rdx` decrements register %rdx by the value in %rax. (It helps to read the instruction as “Subtract `%rax` from `%rdx`.”) 

The first operand can be either an immediate value, a [[register]], or a memory location. The second can be either a register or a memory location. As with the [[MOV|MOV]] instructions, the two operands cannot both be memory locations. Note that when the second operand is a memory location, the processor must read the value from memory, perform the operation, and then write the result back to memory.

> This syntax is reminiscent of the C assignment operators, such as `x -= y`