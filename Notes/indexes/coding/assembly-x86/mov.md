There are many different data movement instructions, differing in their source and destination types, what conversions they perform, and other side effects they may have.

We group the many different instructions into *instruction classes*, where the instructions in a class perform the same operation but with different operand sizes.
# MOV
The `mov` class instructions copy data from a source location to a destination location, without any transformation. The class consists of four instructions:
- `movb`
- `movw`
- `movl`
- `movq`
These four have similar effects; they differ primarily in that they operate on data of different sizes: 1, 2, 4, and 8 bytes, respectively.
![[Pasted image 20250818123607.png|450]]
The *source operand* designates a value that is immediate, stored in a [[register]], or stored in memory
The *destination operand* designates a location that is either a register or a memory address. 

Copying a value from one memory location to another requires two instructions:
-  The first to load the source value into a register
- The second to write this register value to the destination

For most cases the `MOV` instructions will only update the specific register bytes or memory locations indicated by the destination operand. The only exception is that when `movl` has a register as the destination, it will also set the high-order 4 bytes of the register to 0.
![[Pasted image 20250818124727.png]]
A final instruction *`movabsq`* is for dealing with 64-bit immediate data. The regular `movq` instruction can only have immediate source operands that can be represented as 32-bit [[2.2.3 Two's-Complement Encodings|two's complement]] numbers. This value is then sign extended to produce the 64-bit value for the destination. The `movabsq` instruction can have an arbitrary 64-bit immediate value as its source operand and can only have a register as a destination

# MOVZ
This class is used when copying a smaller source value (in a register or memory address) to a larger destination([[register]]).
![[Pasted image 20250818133441.png]]
Instructions in the `MOVZ` class fill out the remaining bytes of the destination with zeros, while those in the `MOVS` class fill them out by sign extension, replicating copies of the most significant bit of the source operand.

Each instruction name has size designators as its final two characters, the first specifying the source size, and the second specifying the destination size.

Note the absence of an explicit instruction to zero-extend a 4-byte source value to an 8-byte destination, this doesn't exist. Instead, this type of data movement can be implemented using a `movl` instruction having a register as the destination.

# MOVS
![[Pasted image 20250818134520.png]]
Figure above also documents the `ctlq` instruction. This instruction has no operands, it always uses [[register]]  `%eax` as its source and `%rax` as the destination for the sign-extended result. It has the exact same effect as the instruction `movslq %eax, %rax`, but it has a more compact encoding.

> x86 imposes the restriction that a `move` instruction cannot have both operands refer to memory locations


> [!NOTE] *Understanding how data movement changes a destination register*
> There are two different conventions regarding wheter and how data movement instructions modify the upper bytes of a destination register. This distinction is illustrated by the following code sequence:
> ![[Pasted image 20250818135026.png]]
> Instruction on line 1 initializes register `%rax` to the pattern `0x0011223344556677`. The remaining instructions have immediate value `-1(0xFF...F)` as their source values.
> The `movb` instruction on line 2 sets the low-order byte of `%rax` to `FF`, while the `movw` instruction on line 3 sets the low-order 2 bytes to `FFFF`, with the remaining bytes unchanged. The `movl` instruction on line 4 sets the low-order 4 bytes to `FFFFFFFF`, but it also sets the high-order 4 bytes to `00000000`. Finally, the `movq` instruction on line 5 sets the complete register to `FFFFFFFFFFFFFFFF`.


> [!NOTE] *Comparing byte movement instructions*
> The following example illustrates how different data movement instructions either do or do not change the high-order bytes of the destination. Observe that the three byte-movement instructions `movb`, `movsbq`, and `movzbq` differ from each other in subtle ways.
> ![[Pasted image 20250818141612.png]]
> The first two lines of the code initialize registers `%rax` and `%dl` to `0011223344556677` and `AA`, respectively. The remaining instructions all copy the low-order byte of `%rdx` to the low-order byte of `%rax`. The `movb` instruction (line 3) does not change the other bytes. The `movsbq` instruction (line 4) sets the other 7 bytes to either all ones or all zeros depending on the high-order bit of the source byte. Since hexadecimal `A` represents binary value `1010`, sign extension causes the higher-order bytes to each be set to `FF`. The `movzbq` instruction (line 5) always sets the other 7 bytes to zero.
