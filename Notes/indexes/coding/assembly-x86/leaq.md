The *load effective address* instruction `leaq` is actually a variant of the [[MOV#MOV|movq]] instruction. It has the form of an instruction that reads from memory to a register but it does not reference memory at all.

| *Instruction*         | *Effect*            | *Description*          |
| --------------------- | ------------------- | ---------------------- |
| `leaq`         $S, D$ | $D \leftarrow  \&S$ | Load effective address |
Its first operand appears to be a memory reference, but instead of reading from the designated location, the instruction copies the effective address to the destination.

This instruction can be used to generate [[Pointers]] for later memory references. 


> [!NOTE] *Arithmetic operations with `leaq`* 
   In addition, it can be used to compactly describe common arithmetic operations. For example, if register `%rdx` contains value $x$, then the instruction `leaq 7(%rdx,%rdx,4), %rax` will set register `%rax` to $5x + 7$. 
   Compilers often find clever uses of `leaq` that have nothing to do with effective address computations. The destination operand must be a [[register]].

---

As an illustration of the use of `leaq` in compiled code, consider the following C program:
```C
long scale(long x, long y, long z) {
	long t = x + 4 * y + 12 * z;
	return t;
}
```
When compiled, the arithmetic operations of the function are implemented by a sequence of three `leaq` functions:
```assembly
//x in %rdi, y in %rsi, z in %rdx

scale:
  leaq    (%rdi, %rsi, 4),   %rax          // x + 4*y
  leaq    (%rdx, %rdx, 2),   %rdx          // z + 2*z = 3*z
  leaq    (%rax, %rdx, 4),   %rax          // (x+4*y) + 4*(3*z) = x + 4*y + 12*z
```

The ability of the `leaq` instruction to perform addition and limited forms of multiplication proves useful when compiling simple arithmetic expressions such as this example.