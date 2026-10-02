# ADD Instruction – EFLAGS Analysis

## Program 1: add1.asm

### Operation

The program adds two 8-bit values:

120 + 10 = 130

The result is stored in AL.

The binary representation of 130 is:

10000010

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | 0 | There was no carry beyond the 8-bit range because 130 fits within 0–255. |
| ZF | 0 | The result is not zero. |
| SF | 1 | The most significant bit of the result is 1. |
| PF | 1 | The result contains an even number of 1 bits. |
| AF | 1 | A carry occurred from bit 3 to bit 4. |
| OF | 1 | 130 is outside the signed 8-bit range of -128 to +127. |

GDB showed:

EFLAGS = 0xA96 [PF AF SF IF OF]

The IF flag is unrelated to the arithmetic operation.

---

## Program 2: add2.asm

### Operation

The program adds two 16-bit values:

32000 + 500 = 32500

The result is stored in AX.

The hexadecimal representation of the result is:

7EF4

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | 0 | 32500 fits within the unsigned 16-bit range of 0–65535. |
| ZF | 0 | The result is not zero. |
| SF | 0 | The most significant bit of the result is 0. |
| PF | 0 | The low byte of the result has odd parity. |
| AF | 0 | No auxiliary carry occurred. |
| OF | 0 | 32500 is within the signed 16-bit range of -32768 to +32767. |

GDB showed:

EFLAGS = 0x202 [IF]

The IF flag is unrelated to the arithmetic operation.

---

## Conclusion

The two ADD programs demonstrate that different arithmetic results produce different EFLAGS statuses.

In add1.asm, the result causes signed overflow, setting OF, while in add2.asm the result fits within both the signed and unsigned 16-bit ranges.