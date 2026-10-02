# SUB Instruction – EFLAGS Analysis

## Program 1: sub1.asm

### Operation

The program subtracts two 8-bit values:

50 - 80 = -30

Since the result is stored in an 8-bit register, the result is represented in two's complement:

-30 = 11100010

This is 226 when interpreted as an unsigned value.

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | 1 | A borrow was required because 50 is less than 80 when treated as unsigned values. |
| ZF | 0 | The result is not zero. |
| SF | 1 | The most significant bit of the 8-bit result is 1. |
| PF | 1 | The low byte 11100010 contains four 1-bits, which is even parity. |
| AF | 0 | No borrow occurred from bit 4 because the lower nibble of 50 (2) is not less than the lower nibble of 80 (0). |
| OF | 0 | -30 is within the signed 8-bit range of -128 to +127. |

GDB showed:

EFLAGS = 0x287 [CF PF SF IF]

The IF flag is unrelated to the subtraction operation.

---

## Program 2: sub2.asm

### Operation

The program subtracts two 16-bit values:

1000 - 2000 = -1000

The 16-bit two's complement representation of -1000 is:

1111110000011000

In hexadecimal:

FC18

GDB displays this as the unsigned value:

64536

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | 1 | A borrow was required because 1000 is less than 2000 when treated as unsigned values. |
| ZF | 0 | The result is not zero. |
| SF | 1 | The most significant bit of the 16-bit result is 1. |
| PF | 1 | The low byte 00011000 contains two 1-bits, which is even parity. |
| AF | 0 | No borrow occurred from bit 4 because the lower nibble of 1000 (8) is not less than the lower nibble of 2000 (0). |
| OF | 0 | -1000 is within the signed 16-bit range of -32768 to +32767. |

GDB showed:

EFLAGS = 0x287 [CF PF SF IF]

The IF flag is unrelated to the subtraction operation.

---

## Conclusion

The SUB programs demonstrate how subtraction affects the EFLAGS register.

Both programs produce negative signed results, so SF is set. Both also require an unsigned borrow, so CF is set. However, neither result causes signed overflow, so OF remains clear.