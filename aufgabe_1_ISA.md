### Aufgabe zur Instruction Set Architecture

Rufen Sie den [RiscV Simulator Ripes](https://ripes.me/) auf.  
Stellen Sie diesen (kleines Chip Symbol in der Menüleiste) auf **"Single cycle processor"** um.  
Stellen Sie in **View** um auf **"Show processor signal values"**.

Fügen Sie diesen Code in der Editor-Ansicht ein:

```asm
_start:
    nop                 # NOP: macht nichts.

    sub  s2, s1, s0     # Register-Operation: s2 = s1 - s0.
                        # 32 Bit Instruktion.

    addi x5, t1, 10     # Add Immediate: x5 = t1 + 10.
                        # Standard 32 Bit, aber evtl. vom Assembler komprimiert.
label:
    addi x5, a4, 1      # Add Immediate: x5 = a4 + 1.
    lui  x6, 0x12345    # Load Upper Immediate:
    jal  x1, label      # Jump and Link: springt zu "label".
                        # Speichert die Rücksprungadresse in x1 (ra).
```
### Anleitung

- Steppen Sie durch den Programmcode, indem Sie auf den **> Knopf** drücken.  
- Wenn im Editor **View mode** auf **Binary** gestellt ist, sehen Sie sowohl die **Binäre** als auch die **Hex-Darstellung** des Assembler-Codes.  
- Wie Sie sehen können, haben die verschiedenen Instruktionen **unterschiedliche Längen** (2 oder 4 Byte).  
- **Hinweis:** Durch den **grünen Play-Knopf** läuft das Programm für die angegebene Zeit.  
- Mit den **> / < Tasten** können Sie instruktionsweise vor- und zurückspringen.  