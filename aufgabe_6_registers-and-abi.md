### Aufgabe zu registers und dem abi standard

In dieser Aufgabe wird gezeigt, wie Register zur Übergabe von Paramdtern zwischen Funktionen genutzt werden können, sowie Registerwerte korrekt von dem Calle gesaved werden. Hierzu wird der Stack eingesetzt, wenn Sie noch nicht wissen was der Stack ist, kann das etwas verwirrend sein. Kommen Sie dazu vielleicht später nochmal zu der Aufgabe zurück.

### Anleitung
- Fügen Sie den Programmcode in [Ripes](https://ripes.me/) in den Editor ein.
- Versuchen Sie zu verstehen was passiert
- Steppen Sie durch das Programm und beobachten Sie wie sich die Registerwerte Ändern.
- Gehen Sie in die Cache Ansicht und schauen Sie was im Cache passiert (Spannend ab Zeile 32)

```asm

    .globl _start
_start:
    # ---------------------------
    # Hauptprogramm
    # ---------------------------

    # Beispiel: Werte für Berechnung in Caller-saved Register setzen
    li a0, 5       # arg_1
    li a1, 10      # arg_2
    
    # Diese Register solle nicht durch die Unterfunktion verändert werden
    li s2, 0xaa
    li s3, 0xbb
    li s4, 0xcc
    li s5, 0xdd

    # Aufruf der Unterfunktion
    # Pseudocode Darstellung:
    # arg_1 = sub_func(arg_1, arg_2)
    jal ra, sub_func

    # Programm beenden
    li a7, 10      # ecall: exit
    ecall

# ---------------------------
# Unterfunktion
# ---------------------------
sub_func:
    # Callee-saved Register sichern (falls benutzt)
    addi sp, sp, -32    # mache platz auf stack für 8x4 bytes
    sw s0, 0(sp)
    sw s1, 4(sp)
    sw s2, 16(sp)
    sw s3, 20(sp)
    sw s4, 24(sp)
    sw s5, 28(sp)
    
    # Unterfunktion lädt andere Werte in die gesicherten Register
    li s2, 0x11
    li s3, 0x22
    li s4, 0x33
    li s5, 0x45

    # Beispiel: Berechnung a0 + a1 -> a0 zurückgeben
    add s0, a0, a1   # s0 = a0 + a1

    # Wert zurück in a0 schreiben
    mv a0, s0

    # Callee-saved Register wiederherstellen
    lw s0, 0(sp)
    lw s1, 4(sp)
    lw s2, 16(sp)
    lw s3, 20(sp)
    lw s4, 24(sp)
    lw s5, 28(sp)
    addi sp, sp, 32    # verwerfe lokale variablen auf stack

    # Zurückkehren
    ret    # Rücksprungadresse in ra (x1)
```

<details>
<summary>💡 Lösung / Hinweise anzeigen</summary>
Beim Durchsteppen des Programms passiert Folgendes:

1. Aufrufende Funktion (_start)
- Die Argumente für die Unterfunktion werden gemäß RISC-V Calling Convention in den Argument-Register a0 und a1 geschrieben (5 und 10).
- Zusätzlich werden einige Callee-saved Register (s2–s5) mit Werten belegt (0xaa, 0xbb, 0xcc, 0xdd). Diese Werte sollen durch die Unterfunktion nicht verändert werden, deshalb muss die Unterfunktion dafür sorgen, sie zu sichern.
- Der Aufruf erfolgt mit jal ra, sub_func. Dabei wird die Rücksprungadresse automatisch in ra (Return Address, x1) geschrieben.
2.	Unterfunktion (sub_func)
- Zuerst legt die Funktion Platz auf dem Stack an: ```addi sp, sp, -32```
- Der Stack Pointer sp zeigt jetzt auf einen reservierten Bereich von 32 Bytes im Speicher.
- Anschließend sichert die Funktion die Inhalte der Callee-saved Register (s0–s5) mit sw (store word) auf dem Stack. Damit sind ihre ursprünglichen Werte geschützt.
- Danach lädt die Funktion neue Werte in s2–s5 (0x11, 0x22, 0x33, 0x45). Hier sieht man schön: obwohl diese Register überschrieben werden, gehen die ursprünglichen Werte nicht verloren, da sie ja auf dem Stack gespeichert wurden.
- Die Berechnung erfolgt: 
```
add s0, a0, a1   # s0 = a0 + a1 = 5 + 10 = 15
mv a0, s0        # Rückgabewert in a0
```
- Ergebnis 15 steht nachher in a0 und wird somit an den Aufrufer zurückgegeben.
3.	Wiederherstellen der Register und Rückkehr
- Die zuvor gesicherten Register werden mit lw (load word) vom Stack zurückgeholt. Jetzt enthalten s2–s5 wieder ihre alten Werte (0xaa, 0xbb, 0xcc, 0xdd).
- Der Stack Pointer wird wieder auf seine alte Position gesetzt (addi sp, sp, 32).
- Mit ret kehrt das Programm über die in ra gespeicherte Adresse zur aufrufenden Funktion zurück.
4.	Nach Rückkehr im Hauptprogramm 
- a0 enthält jetzt das Ergebnis der Berechnung (15).
- Die Register s2–s5 haben weiterhin ihre ursprünglichen Werte – die Calling Convention wurde eingehalten.
</details>
