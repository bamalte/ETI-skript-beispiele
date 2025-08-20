### Aufgabe zu addressing modes

In dieser Aufgabe lernen Sie, wie RISC-V Daten über verschiedene Addressing Modes lädt und speichert.

```asm
    .data
val1:   .word 0xa           # Speicherwert a
val2:   .word 0xb           # Speicherwert b
val3:   .word 0xc           # Speicherwert c
ptr:    .word 0x10000010    # adresse von value    
value:  .word 0xabcdef00    # Speicherwert

    .text
    .globl _start
_start:

    # ----------------------------
    # Immediate Addressing
    # ----------------------------
    addi x1, x0, 5         # x1 = 5

    # ----------------------------    
    # Absolute Addressing (nur über Register möglich)
    # ----------------------------    
    li x3, 0x10000008            # x3 enthält absolute Adresse
    lw x4, 0(x3)                 # lade Wert von dieser Adresse

    # ----------------------------
    # Register Addressing
    # ----------------------------
    add x2, x1, x1         # x2 = x1 + x1 = 10

    # ----------------------------
    # Register-Indirect Addressing (Zeiger auf value) 
    # (nicht ganz korrekt da nicht möglich in Ripes aber die beste Näherung)
    # ----------------------------
    la x3, ptr            # x3 = Adresse von ptr
    lw x4, 0(x3)          # x4 = Inhalt von ptr = Adresse von value
    lw x5, 0(x4)          # x5 = Wert von value = 0xabcdef00

    # ----------------------------
    # Base + Offset Addressing
    # ----------------------------
    la x5, val1             # x5 = Adresse von val1
    lw x6, 0(x5)            # x6 = val1 = a
    lw x7, 4(x5)            # x7 = val2 = b (Offset = 4 Bytes)
    lw x8, 8(x5)            # x8 = val3 = c (Offset = 8 Bytes)

    # ----------------------------
    # Programm beenden
    # ----------------------------
    li a7, 10
    ecall
```

### Anleitung
1.	Öffnen Sie den RISC-V Simulator oder die Ansicht Processor.
2.	Steppen Sie durch den Programmcode, indem Sie auf den > Knopf klicken.
3.	Beobachten Sie die Registerwerte (x1–x5) nach jedem Befehl.
4.	Überlegen Sie:
- Wie verändert sich der Inhalt von Registern beim den unterschiedlichen Addressing-Varianten?
- Es können nicht alle Addressing-Varianten in direkt RiscV realisiert werden, daher sind die obigen Beispiele Annäherungen
<details>
<summary>💡 Lösung / Hinweise anzeigen</summary>
- Immediate Addressing: Der Wert wird direkt in ein Register geschrieben.
- Register Addressing: Operationen erfolgen direkt zwischen Registern.
- Base + Offset Addressing: Lade-/Speicherbefehle verwenden eine Basisadresse in einem Register plus einen konstanten Offset.
- Register-Indirect Addressing: Das Ziel des Speicherzugriffs steht in einem Register (evtl. mit Offset).
- Absolute Addressing: RISC-V unterstützt keine direkte absolute Adressierung; man muss die Adresse in ein Register laden (la) und dann über Base+Offset zugreifen
- Indirect Addressing: Über einen Zeiger im Speicher (Registerinhalt = Adresse) auf den Wert zugreifen.
</details>
