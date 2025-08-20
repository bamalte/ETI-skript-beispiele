### Aufgabe: Logische Operationen in RISC-V

In dieser Aufgabe üben Sie **logischen Instruktionen** aus der RISC-V Basis-ISA. 

---

### Anleitung
1. Kopieren Sie den folgenden Code in [Ripes](https://ripes.me/).  
2. Steppen Sie durch das Programm und beobachten Sie, wie sich die Registerwerte verändern.  
3. Nutzen Sie die Ansicht **Editor** (Sie können unten rechts auch das Zahlenformat der Register zu binary ändern).
4. Beobachten Sie was die Instruktionen machen

---

### Programmcode

```asm
    .text
    .globl _start
_start:
    # Ausgangswerte setzen
    li x1, 0b1100        # rs1 = 12 (0xC)
    li x2, 0b1010        # rs2 = 10 (0xA)

    # ---------------------
    # R-Format Logik
    # ---------------------
    and x3, x1, x2       # AND: 0b1100 & 0b1010 = 0b1000 (8)
    or  x4, x1, x2       # OR : 0b1100 | 0b1010 = 0b1110 (14)
    xor x5, x1, x2       # XOR: 0b1100 ^ 0b1010 = 0b0110 (6)

    sll x6, x1, x2       # Shift left logical (rs2 bits)
    srl x7, x1, x2       # Shift right logical (rs2 bits)
    sra x8, x1, x2       # Shift right arithmetic (rs2 bits)

    # ---------------------
    # I-Format Logik (Immediate)
    # ---------------------
    andi x9,  x1, 0b0110   # ANDI: rs1 & imm
    ori  x10, x1, 0b0001   # ORI : rs1 | imm
    xori x11, x1, 0b1111   # XORI: rs1 ^ imm

    slli x12, x1, 2        # Shift left by 2
    srli x13, x1, 2        # Shift right logical by 2
    srai x14, x1, 2        # Shift right arithmetic by 2

    # Programm beenden
    li a7, 10
    ecall
```
<details>
<summary>💡 Lösung / Hinweise anzeigen</summary>

- R-Format Instruktionen (and, or, xor, sll, srl, sra) nutzen zwei Register (rs1, rs2).
- I-Format Instruktionen (andi, ori, xori, slli, srli, srai) nutzen ein Register (rs1) und einen unmittelbaren Wert (imm).
- Bei Shift-Operationen im R-Format gibt rs2 die Anzahl der zu verschiebenden Bits an, im I-Format ein shamt-Feld (Immediate).
- Beobachten Sie die Unterschiede zwischen logischem Schieben (auffüllen mit 0) und arithmetischem Schieben (Vorzeichenbit bleibt erhalten).
</details>
