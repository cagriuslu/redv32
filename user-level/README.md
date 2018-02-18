# User-Level Instructions

### Instruction Structure

Instructions are either 16-bits or 32-bits wide. 16-bit instructions must be packed together to form 32-bit packs. This is because beginning of an instruction must be aligned to 4 byte boundary on the memory. `nop` instruction can be used for alignment purposes.

##### 16-bit Instructions

Different fields of the instruction is given in the following tables.

|First Byte|[7-0]|
|-|-|
||Opcode|

|Second Byte|[7-4]|[3-2]|[1-0]|
|-|-|-|-|
||Register|Stack 1|Stack 2|

Opcode is an 8 bit number given to different instructions. Register field is used to encode the register for instructions that use a register. Stack 1 and Stack 2 fields are used to encode the stacks for instructions that use stacks.

##### 32-bit Instructions

Different fields of the instruction is given in the following tables.

|First Byte|[7-0]|
|-|-|
||Opcode|

|Second Byte|[7-4]|[3-2]|[1-0]|
|-|-|-|-|
||Register|Stack 1|Stack 2|

|Third Byte|[7-0]|
|-|-|
||Operand (low)|

|Forth Bytes|[7-0]|
|-|-|
||Operand (high)|

Opcode is an 8 bit number given to different instructions. Register field is used to encode the register for instructions that use a register. Stack 1 and Stack 2 fields are used to encode the stacks for instructions that use stacks. Operands in third and forth bytes form a 16-bit number. Operand is in little-endian format.

### Instructions

Opcode|Instruction|Description|Effected Flags
-|-|-|-
0x00| `nop` | No operation | -
0x01| `trap` | Causes a trap, transferring control to the operating system. After the trap is served, execution continues with the next instruction. | -
0x02| `pushv %S` | Decrements `vi` by 4 and pops from `%S` and pushes it to variable stack. | Z, N
0x03| `popv %S` | Pops from variable stack and pushes to `%S` and increments `vi` by 4. | Z, N
0x04| `loadl %S U16` | Sets the low 16-bits of the top of stack `%S` to U16. | Z, N
0x05| `loadh %S U16` | Sets the high 16-bits of the top of stack `%S` to U16. | Z, N
0x06| `push %S [%R I16]` | Pushes the contents of the memory with address `%R + I16 + C` to stack `%S`. | C, Z, V, N
0x07| `push %S [%R %S]` | Pushes the contents of the memory with address `%R + %S + C` to stack `%S`. | C, Z, V, N
0x08| `push %S %R` | Pushes the contents of `%R` to stack `%S`. | Z, N
0x09| `pop %S [%R I16]` | Pops from stack `%S` to memory with address `%R + I16 + C`. | C, Z, V, N
0x0A| `pop %S [%R %S]` | Pops from stack `%S` to memory with address `%R + %S + C`. | C, Z, V, N
0x0B| `pop %S %R` | Pops from stack `%S` to `%R`. | Z, N
0x0C| `add %S I16` | Adds `I16 + C` to the top item of stack `%S`. | C, Z, V, N
0x0D| `add %S` | Adds the top two items of stack `%S` with `C`. | C, Z, V, N
0x0E| `and %S` | Logical-AND the top two items of stack `%S`. | Z, N
0x0F| `or %S` | Logical-OR the top two items of stack `%S`. | Z, N
0x10| `xor %S` | Logical-XOR the top two items of stack `%S`. | Z, N
0x11| `shfl %S` | Shifts the top item of stack `%S` to left by feeding `C` from right, and transfering left-most bit to `C`.| C, Z, N
0x12| `shfr %S` | Shifts the top item of stack `%S` to right by feeding `C` from left, and transfering right-most bit to `C`.| C, Z, N
0x13| `skipc` | Skips the next instruction if `C` is set, else skips the subsequent instruction. | -
0x14| `skipz` | Skips the next instruction if `Z` is set, else skips the subsequent instruction. | -
0x15| `skipv` | Skips the next instruction if `V` is set, else skips the subsequent instruction. | -
0x16| `skipn` | Skips the next instruction if `N` is set, else skips the subsequent instruction. | -
0x17| `setc` | Sets the `C` flag. | C
0x18| `clrc` | Clears the `C` flag. | C

`%S` is one of the following: `%sa` `%sb` `%sc` `%sd`\
`%R` is one of the following: `%ia` `%ib` `%ic` `%id` `%vi` `%pc` `%zr` `%sr` `%ra` `%rb` `%rc` `%rd` `%re` `%rf` `%rg` `%rh`\
`U16` is an 16-bit unsigned number.\
`I16` is an 16-bit signed number.
`C` is the carry flag.
`Z` is the zero flag.
`V` is the overflow flag.
`N` is the negative flag.
