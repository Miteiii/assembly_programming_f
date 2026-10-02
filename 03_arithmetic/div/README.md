# DIV Instruction – EFLAGS Analysis

## Program 1: div1.asm

### Operation

The program performs unsigned division using the `DIV` instruction.

The divisor is stored in BL.

The `DIV` instruction divides the value in AX by the 8-bit divisor.

For an 8-bit division:

- Quotient → AL
- Remainder → AH

The quotient and remainder can therefore be inspected using the AL and AH registers.

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | Undefined | DIV does not define the Carry Flag. |
| ZF | Undefined | DIV does not define the Zero Flag. |
| SF | Undefined | DIV does not define the Sign Flag. |
| PF | Undefined | DIV does not define the Parity Flag. |
| AF | Undefined | DIV does not define the Auxiliary Carry Flag. |
| OF | Undefined | DIV does not define the Overflow Flag. |

GDB showed:

EFLAGS = 0x212 [AF IF]

The displayed AF and IF values should not be interpreted as results of the DIV operation because DIV leaves the arithmetic flags undefined.

---

## Program 2: div2.asm

### Operation

The program performs unsigned division using a 16-bit divisor.

For 16-bit `DIV`:

- Dividend → DX:AX
- Divisor → BX
- Quotient → AX
- Remainder → DX

The program therefore uses DX:AX as the dividend and BX as the divisor.

After the division, GDB showed:

AX = 166

DX = 200

BX = 300

Therefore, the quotient is stored in AX and the remainder is stored in DX.

### EFLAGS Analysis

| Flag | Status | Explanation |
|------|--------|-------------|
| CF | Undefined | DIV does not define the Carry Flag. |
| ZF | Undefined | DIV does not define the Zero Flag. |
| SF | Undefined | DIV does not define the Sign Flag. |
| PF | Undefined | DIV does not define the Parity Flag. |
| AF | Undefined | DIV does not define the Auxiliary Carry Flag. |
| OF | Undefined | DIV does not define the Overflow Flag. |

GDB showed:

EFLAGS = 0x212 [AF IF]

The displayed AF and IF values should not be interpreted as results of the DIV operation because DIV leaves the arithmetic flags undefined.

---

## Conclusion

The DIV programs demonstrate that the `DIV` instruction produces a quotient and remainder rather than defined arithmetic flag results.

For 8-bit division, the quotient is stored in AL and the remainder in AH.

For 16-bit division, the quotient is stored in AX and the remainder in DX.

Unlike ADD and SUB, DIV does not define CF, ZF, SF, PF, AF, or OF. Therefore, the flag values displayed by GDB after DIV should not be used to describe the result of the division.