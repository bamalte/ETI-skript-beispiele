### Aufgabe zu addressing modes

In dieser Aufgabe lernen Sie, wie RISC-V Daten über verschiedene Addressing Modes lädt und speichert.

```asm
    .data
val1:   .word 10           # Speicherwert 10
val2:   .word 20           # Speicherwert 20
ptr:    .word value        # Zeiger auf value
value:  .word 0xffffff00   # Speicherwert

    .text
    .globl _start
_start:

    # Immediate Addressing: x1 = 5
    addi x1, x0, 5           # x1 = 5

    # Absolute Addressing (nur über Register möglich)
    la x3, 0x10010000        # x3 enthält absolute Adresse
    lw x4, 0(x3)             # lade Wert von dieser Adresse

    # Indirect Addressing (über Zeiger)
    la x3, ptr                # x3 = Adresse von ptr
    lw x4, 0(x3)              # x4 = Inhalt von ptr (Adresse von value)

    # Register Addressing: x2 = x1 + x1
    add x2, x1, x1           # x2 = x1 + x1 = 10

    # Register-Indirect Addressing:
    la x1, value             # x1 enthält die Adresse von value
    lw x2, 0(x1)             # x2 = *x1 = 0xffffff00

    # Base + Offset Addressing:
    la x3, val1               # x3 = Adresse von val1
    lw x4, 0(x3)              # x4 = val1 = 10
    lw x5, 4(x3)              # x5 = val2 = 20 (4 Bytes Offset)

    # Programm beenden
    li a7, 10
    ecall
```

### Anleitung
1.	Öffnen Sie den RISC-V Simulator oder die Ansicht Processor.
2.	Steppen Sie durch den Programmcode, indem Sie auf den > Knopf klicken.
3.	Beobachten Sie die Registerwerte (x1–x5) nach jedem Befehl.
4.	Überlegen Sie:
- Wie verändert sich der Inhalt von Registern beim Immediate- und Register-Addressing?
- Wie unterscheiden sich Base+Offset und Register-Indirect Addressing?
- Wie kann man „Absolute Addressing“ in RISC-V realisieren?

<details>
<summary>💡 Lösung / Hinweise anzeigen</summary>

- Immediate Addressing: Der Wert wird direkt in ein Register geschrieben.
- Register Addressing: Operationen erfolgen direkt zwischen Registern.
- Base + Offset Addressing: Lade-/Speicherbefehle verwenden eine Basisadresse in einem Register plus einen konstanten Offset.
- Register-Indirect Addressing: Das Ziel des Speicherzugriffs steht in einem Register (evtl. mit Offset).
- Absolute Addressing: RISC-V unterstützt keine direkte absolute Adressierung; man muss die Adresse in ein Register laden (la) und dann über Base+Offset zugreifen.
- Indirect Addressing: Über einen Zeiger im Speicher (Registerinhalt = Adresse) auf den Wert zugreifen.
</details>
