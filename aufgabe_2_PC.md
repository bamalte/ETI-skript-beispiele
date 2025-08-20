### Aufgabe zum Program Counter

Nutzen Sie die gleichen Einstellungen wie in [Aufgabe 1](https://github.com/bamalte/ETI-skript-beispiele/blob/main/aufgabe_1_ISA.md).

---

### Anleitung

1. Wechseln Sie zur Ansicht **Processor**.  
2. Steppen Sie durch den Programmcode, indem Sie auf den **> Knopf** drücken.  
3. Beobachten Sie den Wert des **Program Counters (PC)**.  
   - Falls der Wert des PCs nicht angezeigt wird, überprüfen Sie die Einstellungen aus **Aufgabe 1**.  
4. Überlegen Sie: **Warum wird der PC immer um 4 erhöht?**

---

<details>
<summary>💡 Lösung anzeigen</summary>

Der **Program Counter (PC)** zeigt auf die Adresse der aktuellen Instruktion.  
Im Standard-RISC-V ohne Compressed-Extension (RVC) sind alle Instruktionen **32 Bit = 4 Byte** lang.  
Daher erhöht sich der PC nach jedem Befehl um **4**, damit er auf die nächste Instruktion zeigt.  

*(Hinweis: Der PC zeigt auf eine Instruktion im Instr. memory, dementpsrechend können Sie am Ausgang des Instr. memorys jetzt auch die Instrukionen aus Aufgabe 1 sehen.)*

</details>