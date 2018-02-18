# Machine Specifications

`redv32` has 4 operand stacks, one variable stack, and 16 registers. Every item in stacks and registers are 32-bits wide.

### Operand Stacks

There are 4 operand stacks inside the machine. Each stack can hold 8 32-bit numbers. Arithmetic and logic operations are done on operand stacks. The operand stacks are named `sa`, `sb`, `sc`, and `sd`.

If no items are pushed to an operand stacks, popping from it produces zero value. If more items are push to a full operand stack, the earliest pushed item is dropped.

### Variable Stack

There is one variable stack that resides on the memory. Variable stack is used to store local variables and to pass arguments to functions. `vi` register holds the address of the last pushed item to the stack. When `pushv` and `popv` instructions are used, `vi` is automatically decremented and incremented. First 8192 items (32768 bytes) of the variable stack can be accessed randomly using `push` and `pop` instructions.

### Registers

There are 16 registers inside the machine. Their purposes are summarized in the following table.

Name | Purpose
-----|-----
`ia` | Index Register
`ib` | Index Register
`ic` | Index Register
`id` | Index Register
`vi` | Variable Stack Index
`pc` | Program Counter
`zr` | Zero register
`sr` | Status Register
`ra` | Undefined
`rb` | Undefined
`rc` | Undefined
`rd` | Undefined
`re` | Undefined
`rf` | Undefined
`rg` | Undefined
`rh` | Undefined

Index Registers are used as an index while accessing the memory. Variable Stack Index holds the address of the top of the variable stack. Program Counter holds the address of the next instruction to be executed. Zero Register always holds the value zero and write operations to it are ignored. Status Register holds the status flags. Undefined registers are reserver for further use, thus should not be used by programs.

### Status Flags

`sr` register contains 4 flags.

|Bit 3|Bit 2|Bit 1|Bit 0|
|-----|-----|-----|-----|
|Negative (N)|Overflow (V)|Zero (Z)|Carry (C)|

Negative flag is set if the most recent arithmetic or logical operation generated a result with the most significant bit 1.
Overflow flag is set if the most recent arithmetic or logical operation generated an overflow.
Zero flag is set if the most recent arithmetic or logical operation generated a zero value.
Carry flag is set if the most recent arithmetic or logical operation generated a carry.
