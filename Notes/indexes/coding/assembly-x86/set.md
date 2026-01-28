These instructions set a single byte to 0 or 1 depending on some combination of the [[3.6.1 Condition Codes|condition codes]], they differ from one another based on which combinations of condition codes they consider, as indicated by the different suffixes for the instruction names. 
	It is important to recognize that the suffixes for these instructions denote different
conditions and not different [[Operands - as|operand]] sizes. For example, instructions `setl` and `setb` denote “set less” and “set below,” not “set long word” or “set byte.”

A `SET` instruction has either one of the low-order single-byte [[register]] elements or a single-byte memory location as its destination, setting this byte to either 0 or 1, we must also clear the high-order bits.
![[Pasted image 20250821131150.png|570]]
A typical instruction sequence to compute the C expression `a < b`, where `a` and `b` are both of type long, proceeds as follows:
![[Pasted image 20250821131322.png]]
Note the comparison order of the [[CMP & TEST|cmpq]] instruction . Although the arguments are listed in the order `%rsi` (b), then `%rdi` (a), the comparison is really between a and b. Recall also, that the [[MOV#MOVZ|movzbl]] instruction (line 4) clears the upper 4 bytes of the entire register, `%rax`, as well. 

For some of the underlying machine instructions, there are multiple possible names, which we list as “*synonyms*.” For example, both `setg` (for “set greater”) and `setnle` (for “set not less or equal”) refer to the same machine instruction. 
	Compilers and disassemblers make arbitrary choices of which names to use.
Although all arithmetic and logical operations set the condition codes, the descriptions of the different set instructions apply to the case where a comparison instruction has been executed, setting the condition codes according to the computation `t = a-b`. 
	More specifically, let `a`, `b`, and `t` be the integers represented in [[2.2.3 Two's-Complement Encodings|two's complement]]
form by variables `a`, `b`, and `t`, respectively, and so $t = a -_w^t b$, where $w$ depends on the sizes associated with `a` and `b`.

Consider the `sete` instruction, When `a = b`, we will have `t = 0`, and hence the zero flag indicates equality