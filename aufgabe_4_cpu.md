### Aufgabe zu CPU basics 

Nutzen Sie die gleichen Einstellungen wie in [Aufgabe 1](https://github.com/bamalte/ETI-skript-beispiele/blob/main/aufgabe_1_ISA.md).

Fügen Sie diesen Code in der Editor-Ansicht ein:

```asm
_start:
    nop
    li a0, 1
    li a1, 2
    add a2, a0, a1
    jal _start
```
### Anleitung

- Steppen Sie durch den Programmcode
- Schauen Sie sich in der Processor Ansicht an, welche Werte an den Registern und am Ausgang der ALU im Schritt des "adds" anliegen.