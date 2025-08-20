### Aufgabe: R-Format vs. I-Format Instruktionen

In dieser Aufgabe wird der Unterschied zwischen **R-Format** (Register-Register) und **I-Format** (Register-Immediate oder Load) Instruktionen verdeutlicht.

---

### Anleitung
1. Fügen Sie den Programmcode unten in [Ripes](https://ripes.me/) ein.  
2. Wechseln Sie in die **Processor** Ansicht.  
3. Steppen Sie durch den Code und beobachten Sie die Instruktionen im **Instruktionsfeld**.  
4. Vergleichen Sie: Welche Felder werden bei R-Format vs. I-Format Instruktionen belegt?  
   - **R-Format:** `rd, rs1, rs2, funct3, funct7, opcode`  
   - **I-Format:** `rd, rs1, imm[11:0], funct3, opcode`

---

### Programmcode

```asm
    .data
val1:   .word 7
val2:   .word 3

    .text
    .globl _start
_start:

    .data
val1:   .word 7
val2:   .word 3

    .text
    .globl _start
_start:

    # ---------------------------
    # R-Format Instruktionen
    # ---------------------------
    li x1, 5          # Lade 5 in x1
    li x2, 6          # Lade 6 in x2

    add x3, x1, x2    # R-Format: x3 = x1 + x2 = 5 + 6 = 11
    sub x4, x1, x2    # R-Format: x4 = x1 - x2 = -1
    and x5, x1, x2    # R-Format: x5 = 5 & 6

    # ---------------------------
    # I-Format Instruktionen
    # ---------------------------
    addi x6, x1, 10   # I-Format: x6 = x1 + 10 = 15
    ori  x7, x1, 0xf  # I-Format: x7 = x1 | 0xf

    # Load (auch I-Format!)
    la   x8, val1     # Adresse von val1 in x8
    lw   x9, 0(x8)    # I-Format: x9 = Speicher[x8+0] = 7
    
    # Ende des Programms
    li a7, 10
    ecall
```

<details>
<summary>💡 Lösung anzeigen</summary>

- R-Format Instruktionen (z. B. add, sub) kombinieren zwei Registerwerte (rs1, rs2) und schreiben das Ergebnis nach rd.
- I-Format Instruktionen (z. B. addi, ori, lw) kombinieren rs1 mit einem Immediate oder nutzen es als Basisadresse.
- In Ripes können Sie die unterschiedlichen Bitfelder (rd, rs1, rs2 vs. imm) im Instruktionsaufbau ablesen.
</details>