# MUL Instruction – EFLAGS Analysis

## Program 1: mul1.asm

### Operation

The program multiplies two 8-bit unsigned values:

25 × 10 = 250

The `MUL` instruction uses AL as the implicit first operand.

The result is stored in AX because multiplying two 8-bit values produces a 16-bit result.

250 in hexadecimal is:

00FA

Therefore:

AH = 00
AL = FA

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | 0 | The upper half of the result (AH) is 0, so no significant bits were placed in the upper half. |
| OF | 0 | The upper half of the result is 0, so unsigned multiplication did not require the upper half. |
| ZF | Undefined | MUL does not define the Zero Flag. |
| SF | Undefined | MUL does not define the Sign Flag. |
| PF | Undefined | MUL does not define the Parity Flag. |
| AF | Undefined | MUL does not define the Auxiliary Carry Flag. |

GDB showed:

EFLAGS = 0x202 [IF]

The IF flag is unrelated to the multiplication operation.

For `MUL`, only CF and OF have defined results. The other arithmetic flags should not be interpreted from the GDB output.

---

## Program 2: mul2.asm

### Operation

The program multiplies two 16-bit unsigned values:

3000 × 200 = 600000

The `MUL` instruction uses AX as the implicit first operand.

The 32-bit result is stored in DX:AX.

The result is:

600000 = 0009 27C0

Therefore:

DX = 0009
AX = 27C0

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | 1 | The upper half of the result (DX) is non-zero, so the result does not fit entirely in AX. |
| OF | 1 | The upper half of the result (DX) is non-zero. |
| ZF | Undefined | MUL does not define the Zero Flag. |
| SF | Undefined | MUL does not define the Sign Flag. |
| PF | Undefined | MUL does not define the Parity Flag. |
| AF | Undefined | MUL does not define the Auxiliary Carry Flag. |

GDB showed:

EFLAGS = 0x203 [CF IF OF]

The IF flag is unrelated to the multiplication operation.

---

## Conclusion

The two MUL programs demonstrate how CF and OF indicate whether the multiplication result fits completely within the destination register.

For mul1.asm, 25 × 10 produces 250, which fits completely in AL, so CF and OF are cleared.

For mul2.asm, 3000 × 200 produces 600000. The result requires both DX and AX, so CF and OF are set.

The remaining arithmetic flags are undefined after MUL.