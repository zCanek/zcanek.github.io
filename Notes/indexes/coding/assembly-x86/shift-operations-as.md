![[Pasted image 20250820121350.png]]
In [[2.1.9 Shift Operations in C|shift operations]], the shift amount is given first and the value to shift is given second, the different shift instructions can specify the shift amount either as an immediate value or with the single-byte [[register]] `%cl`. (These instructions are unusual in only allowing this specific register as the operand.) 

In principle, having a 1-byte shift amount would make it possible to encode shift amounts ranging up to $2^8 − 1 = 255$. With x86-64, a shift instruction operating on data values that are $w$ bits long determines the shift amount from the low-order $m$ bits of register `%cl`, where $2^m = w$. The higher-order bits are ignored. So, for example, when register %cl has hexadecimal value 0xFF, then instruction `salb` would shift by 7, while `salw` would shift by 15, `sall` would shift by 31, and `salq` would shift by 63. 

There are two names for the left shift instruction: `sal` and `shl`. Both have the same effect, filling from the right with zeros. The right shift instructions differ in that `sar` performs an arithmetic shift (fill with copies of the sign bit), whereas `shr` performs a logical shift (fill with zeros). 

The destination operand of a shift operation can be either a register or a memory location.