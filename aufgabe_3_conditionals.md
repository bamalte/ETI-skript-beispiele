### Aufgabe: Jumps und Branches in RISC-V

In dieser Aufgabe lernen Sie **Jumps** und **Branches** (bedingte Sprünge) kennen.

---

### Aufgabe 1: Jump (bedingungslos)

1. Schreiben Sie ein RISC-V Programm, das mit einem **bedingungslosen Jump** (`JAL`) zu einem Label `loop` springt.  
2. Innerhalb von `loop` fügen Sie einen `NOP` ein und springen danach wieder zurück zum Start.  
3. Beobachten Sie im Simulator ([Ripes](https://ripes.me/)) den **Program Counter**.

<details>
<summary>💡 Lösung</summary>

```asm
_start:
    nop
    nop
    nop
    jal x0, loop   # Jump to 'loop'
    addi x5, t1, 10 # wird niemals ausgeführt

loop:
    nop
    nop
    jal x1, _start # Jump back to start
```
- Der PC wird direkt auf das Label gesetzt, unabhängig von Bedingungen.
</details>

### Aufgabe 1.1:
- Was passiert mit dem Werten in x3 und x1?
  
<details>
<summary>💡 Lösung</summary>
Da der Programmcode bei einem jump in einen anderen Bereich des Programms springt, wurde in der Lösung in x3 und x1 die Rücksprungadressen gespeichert, das ergibt in einem loop nicht unbedingt Sinn, dient daher zur demonstration.

Ein normales Vorgehen könnte sein:

In RISC-V ist standardmäßig das Register x1, auch ra (return address) genannt, für die Rücksprungadresse vorgesehen.
- Bei einem jal x1, label wird die Adresse der nächsten Instruktion in x1 gespeichert.
- Anschließend kann man mit jalr x0, x1, 0 oder ähnlichem zur Rücksprungadresse zurückkehren.

```asm
_start:
    # Hauptprogramm
    li t0, 10        # Beispielwert setzen
    jal x1, subroutine  # Springe zu subroutine und speichere Rückadresse in x1
    addi t0, t0, 5   # Fortsetzung nach Rückkehr

end:
    nop              # Programmende

subroutine:
    addi t0, t0, 1   # Beispieloperation in Subroutine
    jalr x0, x1, 0   # Rücksprung zur Adresse in x1 (x0 als Ziel heißt kein neues x-Register)
```
</details>


### Aufgabe 2: Branch (bedingungslos)
1.	Simulieren Sie einen Branch, der immer ausgeführt wird, z. B. indem Sie eine Bedingung wählen, die immer wahr ist (bge x5, x5, label).
2.	Beobachten Sie, dass der PC auf das Ziel springt, unabhängig von externen Vergleichen.

<details>
<summary>💡 Lösung</summary>

```asm
_start:
    li x5, 0
    bge x5, x5, branch_label  # Branch if greater or equal (immer wahr)
    nop

branch_label:
    nop
```

- Der PC springt immer, da x5 >= x5 immer gilt.
</details>

### Aufgabe 2.2:
1. Wird beim branch auch eine Rücksprungadresse gespeichert?

<details>
<summary>💡 Lösung</summary>
- In einer Übersicht über RiscV Instruktionen wie diese [hier](https://msyksphinz-self.github.io/riscv-isadoc/html/rvi.html#jal) können Sie die Lösung der Beschreibung von jal im Vergleich zu beq entnehmen.
</details>

### Aufgabe 3: Branch (bedingt)
1.	Initialisieren Sie zwei Register, z. B. x5 = 5, x6 = 5.
2.	Schreiben Sie ein RISC-V Programm, das nur springt, wenn x5 == x6 (vergleichbar mit “Jump if Equal”).
3.	Beobachten Sie den PC im Simulator.

<details>
<summary>💡 Lösung</summary>

```asm
_start:
    li x5, 5
    li x6, 5
    beq x5, x6, jump_label  # Bedingter Sprung, springt nur wenn x5 == x6
    nop

jump_label:
    nop
```

- Das ist ein bedingter “Jump” realisiert über eine Branch-Instruktion.
- Der Sprung erfolgt nur, wenn die Bedingung erfüllt ist.
</details>

### Aufgabe 4: Branch 2 (bedingt)
1.	Initialisieren Sie zwei Register, z. B. x5 = 3, x6 = 5.
2.	Verwenden Sie einen Branch (BLT), der nur springt, wenn x5 < x6.
3.	Beobachten Sie, wann der Sprung ausgeführt wird.

<details>
<summary>💡 Lösung</summary>

```asm
_start:
    li x5, 3
    li x6, 5
    blt x5, x6, branch_label  # Branch if x5 < x6
    nop

branch_label:
    nop
```

- BLT springt nur, wenn die Bedingung erfüllt ist.
- Hier wird gesprungen, weil 3 < 5.

</details>

### Aufgabe 5: For-Schleife (bedingter Loop)
1. Initialisieren Sie einen Zähler `t0 = 0` und ein Ergebnisregister `s0 = 0`.
2. Implementieren Sie eine Schleife, die 5-mal durchläuft:
   - Erhöhen Sie den Zähler `t0` um 1.
   - Addieren Sie den aktuellen Zählerwert zu `s0`.
   - Springen Sie zurück zum Schleifenanfang, solange `t0 < 5`.
3. Beobachten Sie, wie sich der Program Counter während der Schleife verändert.

<details>
<summary>💡 Lösung</summary>

```asm
_start:
    li t0, 0        # Schleifenzähler auf 0 setzen
    li s0, 0        # Ergebnisregister auf 0 setzen

loop:
    addi t0, t0, 1  # Zähler erhöhen
    add  s0, s0, t0 # Wert zum Ergebnis addieren
    li   t1, 5      # Schleifenobergrenze
    blt  t0, t1, loop  # Springe zurück, solange t0 < 5

end:
    nop             # Ende der Schleife
```

- t0 speichert den Schleifenzähler, s0 das Ergebnis.
- Die Schleife durchläuft 5 Iterationen.
- Der Program Counter springt bei jedem blt zurück, solange die Bedingung erfüllt ist.
- Am Ende enthält s0 die Summe 1+2+3+4+5 = 15.
<details>
