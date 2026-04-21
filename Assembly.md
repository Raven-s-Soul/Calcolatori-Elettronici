# Guida per Assembly 101

### I file in Assembly estensione .s
```javascript
.SECT .TEXT == istruzioni 
.SECT .DATA == dati
.SECT .BSS  == dati non inizializzati 

! questo è un commento in Assembly
```
<details>
<summary><h2> Macros</h2></summary>

> This is the only macros you need to know.
```assembly
_EXIT = 1
_PRINTF = 127
```
Metti le macro prima di .SECT .TEXT
</details>

***

<details>
<summary><h2>.SECT .TEXT</h2></summary>
<br> 
<details>
<summary><h3>Registri</h3></summary>

- AX = Accumulatore
- SP & BP = Stack usage

### 1. Registri Generali (Dati)
Questi registri possono essere usati interamente (16 bit) o divisi in due parti da 8 bit: **H** (High - Alta) e **L** (Low - Bassa).

| Registro (16 bit) | Descrizione | Utilizzo tipico |
| :--- | :--- | :--- |
| **AX** (AH/AL) | Accumulatore | Operazioni aritmetiche, logiche e I/O. |
| **BX** (BH/BL) | Base | Indirizzamento indiretto della memoria. |
| **CX** (CH/CL) | Conteggio | Cicli (loop) e operazioni ripetitive. |
| **DX** (DH/DL) | Dati | Moltiplicazioni, divisioni e indirizzi I/O. |

---

### 2. Registri di Segmento
Servono a definire i "blocchi" di memoria in cui il programma sta lavorando.

| Registro | Nome Esteso | Funzione |
| :--- | :--- | :--- |
| **CS** | Code Segment | Contiene le istruzioni del programma da eseguire. |
| **DS** | Data Segment | Contiene i dati statici e le variabili globali. |
| **SS** | Stack Segment | Gestisce l'area di memoria "Stack" (pila). |
| **ES** | Extra Segment | Segmento aggiuntivo per operazioni su stringhe o dati extra. |

---

### 3. Puntatori e Indici
Utilizzati per puntare a indirizzi specifici all'interno dei segmenti di memoria.

| Registro | Nome Esteso | Funzione |
| :--- | :--- | :--- |
| **SP** | Stack Pointer | Punta sempre alla "cima" corrente dello Stack. |
| **BP** | Base Pointer | Punta alla base di un blocco di dati nello Stack. |
| **SI** | Source Index | Indice di origine per il trasferimento di dati. |
| **DI** | Destination Index | Indice di destinazione per il trasferimento di dati. |

---

### 4. Registri di Controllo e Stato
Indicano il risultato dell'ultima operazione (Flag) o la prossima istruzione.

| Registro | Descrizione | Dettagli |
| :--- | :--- | :--- |
| **IP / PC** | Instruction Pointer | Contiene l'indirizzo della **prossima istruzione** da eseguire. |
| **SF / CC** | Status Flags | Bit singoli che indicano: Segno (S), Zero (Z), Overflow (O), Carry (C), ecc. |

</details><br>
<details>
<summary><h3>Access Types</h3></summary>

|Notazioni | Spiegazione |
|:--:|:--|
|e| indirizzo effettivo|
|r|registro|
|#|operando immediato {Tipo un numero Dec, Hex, …}|

### 1. Indirizzamento dei Registri (Register Addressing)
Il dato si trova direttamente all'interno di un registro della CPU.

| Tipo | Operando | Esempi |
| :--- | :--- | :--- |
| **Byte register** | Registro a 8 bit | AH, AL, BH, BL, CH, CL, DH, DL |
| **Word register** | Registro a 16 bit | AX, BX, CX, DX, SP, BP, SI, DI |

---

### 2. Indirizzamento del Segmento Dati (Data Segment)
Metodi per calcolare l'indirizzo di un dato situato nella memoria RAM (nel Data Segment).

| Modalità | Come viene calcolato l'indirizzo | Esempio Sintassi |
| :--- | :--- | :--- |
| **Diretto** | L'indirizzo segue il codice operativo | (#) |
| **Indiretto** | L'indirizzo è contenuto in un registro | (SI), (DI), (BX) |
| **Con Spostamento** | Registro + un valore fisso (displacement) | #(SI), #(DI), #(BX) |
| **Con Indice** | BX + Registro Indice (SI o DI) | (BX)(SI), (BX)(DI) |
| **Indice + Spostamento** | BX + SI/DI + Valore fisso | #(BX)(SI), #(BX)(DI) |

---

### 3. Indirizzamento del Segmento Stack (Stack Segment)
Specifico per accedere ai dati salvati temporaneamente nella pila (Stack) usando il Base Pointer.

| Modalità | Come viene calcolato l'indirizzo | Esempio Sintassi |
| :--- | :--- | :--- |
| **BP Indiretto** | L'indirizzo è nel registro BP | (BP) |
| **BP + Spostamento** | Registro BP + Valore fisso | #(BP) |
| **BP + Indice** | BP + Registro Indice (SI o DI) | (BP)(SI), (BP)(DI) |
| **BP + Indice + Spost.** | BP + SI/DI + Valore fisso | #(BP)(SI), #(BP)(DI) |

---

### 4. Dati Immediati e Indirizzi Impliciti
Casi speciali dove il dato è nel codice stesso o l'indirizzo è sottointeso dal comando.

| Categoria | Descrizione / Operando | Istruzioni Esempio |
| :--- | :--- | :--- |
| **Dati Immediati** | Il valore è parte dell'istruzione | MOV AX, **#** (valore) |
| **Stack (Push/Pop)** | Indirizzo indiretto gestito da SP | PUSH, POP, PUSHF |
| **Flags** | Registro di stato | LAHF, STC, CLC |
| **Stringhe** | Usa SI, DI e CX per ripetizioni | MOVS, CMPS, SCAS |
| **Input / Output** | Usa i registri AX o AL | IN #, OUT # |
| **Conversioni** | Cambia formato (da Byte a Word) | CBW, CWD |
</details><br>
<details>
<summary><h3>Funzioni Base</h3></summary>

**Legenda Operandi:**
* `e`: Indirizzo effettivo (memoria o registro)
* `r`: Registro
* `#`: Valore immediato
* `*`: Il flag viene modificato in base al risultato
* `-`: Il flag non viene modificato
* `0`: Flag azzerato
* `U`: Stato del flag indefinito dopo l'operazione
* `CL`: Registro utilizzato per il conteggio degli scorrimenti


### 1. Istruzioni di Trasferimento Dati
Queste istruzioni **non influenzano mai** i flag di stato.

| Mnemonico | Descrizione | Operandi |
| :--- | :--- | :--- |
| **MOV(B)** | Muove dati (Word o Byte) | r ← e, e ← r, e ← # |
| **XCHG(B)** | Scambia i valori tra due operandi | r ↔ e |
| **LEA** | Carica l'indirizzo effettivo | r ← #e |
| **PUSH / POP** | Inserisce/Estrae dallo Stack | e, # (solo push) |
| **PUSHF / POPF** | Inserisce/Estrae i Flag nello Stack | - |
| **XLAT** | Traduce AL usando una tabella in BX | - |

---

### 2. Istruzioni Aritmetiche (Somma e Sottrazione)
Queste istruzioni aggiornano sempre i flag di stato (`*`) per riflettere il risultato.

| Mnemonico | Descrizione | Operandi | O, S, Z, C |
| :--- | :--- | :--- | :---: |
| **ADD(B)** | Somma (Word o Byte) | r ← e, e ← r, e ← # | * |
| **ADC(B)** | Somma con Riporto (Carry) | r ← e, e ← r, e ← # | * |
| **SUB(B)** | Sottrazione | r ← e, e ← r, e ← # | * |
| **SBB(B)** | Sottrazione con Prestito (Borrow) | r ← e, e ← r, e ← # | * |

---

### 3. Istruzioni di Moltiplicazione e Divisione
Queste istruzioni hanno un comportamento più complesso sui flag (spesso diventano indefiniti `U`).

| Mnemonico | Descrizione | Operando | Effetto sui Flag |
| :--- | :--- | :--- | :--- |
| **MUL / IMUL** | Moltiplicazione (senza/con segno) | e | O, C sono impostati; S, Z sono indefiniti (U) |
| **DIV / IDIV** | Divisione (senza/con segno) | e | Tutti i flag (O, S, Z, C) sono indefiniti (U) |

---

### 4. Manipolazione Dati e Aritmetica Unaria
Queste istruzioni lavorano su un singolo operando o preparano i dati per operazioni più ampie.

| Mnemonico | Descrizione | Operandi | O, S, Z, C |
| :--- | :--- | :--- | :---: |
| **CBW / CWD** | Estensione del segno (Byte→Word / Word→Double) | - | - |
| **NEG(B)** | Negazione binaria (Complemento a 2) | e | * |
| **NOT(B)** | Complemento logico (Inversione bit) | e | - |
| **INC(B)** | Incrementa (+1) | e | O, S, Z (C invariato) |
| **DEC(B)** | Decrementa (-1) | e | O, S, Z (C invariato) |

---

### 5. Operazioni Logiche Bitwise
Eseguono operazioni logiche bit a bit. Nota: azzerano sempre i flag di Carry (C) e Overflow (O).

| Mnemonico | Descrizione | Operandi | O | S, Z | C |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **AND(B)** | AND logico | e ← r, r ← e, e ← # | 0 | * | 0 |
| **OR(B)** | OR logico | e ← r, r ← e, e ← # | 0 | * | 0 |
| **XOR(B)** | OR esclusivo (XOR) | e ← r, r ← e, e ← # | 0 | * | 0 |

---

### 6. Scorrimento e Rotazione (Shift & Rotate)
Spostano i bit a destra o a sinistra. Il numero di spostamenti è 1 o contenuto in `CL`.

| Mnemonico | Descrizione | Operandi | O, S, Z, C |
| :--- | :--- | :--- | :---: |
| **SHR / SAR** | Scorrimento a Destra (Logico / Aritmetico) | e ← 1, e ← CL | * |
| **SAL / SHL** | Scorrimento a Sinistra (Aritmetico / Logico) | e ← 1, e ← CL | * |
| **ROL / ROR** | Rotazione a Sinistra / Destra | e ← 1, e ← CL | O, C (S, Z invariati) |
| **RCL / RCR** | Rotazione attraverso il Carry (SX / DX) | e ← 1, e ← CL | O, C (S, Z invariati) |
</details><br>
<details>
<summary><h3>Jumps (JMP)</h3></summary>
  
> Solo i jmp semplici

| Condizione (Destinazione vs Sorgente) | Senza Segno (Unsigned) | Con Segno (Signed) |
| :--- | :---: | :---: |
| **Uguale** (=) | **JE** | **JE** |
| **Diverso** (≠) | **JNE** | **JNE** |
| **Maggiore** (>) | **JA** *(Above)* | **JG** *(Greater)* |
| **Maggiore o Uguale** (≥) | **JAE** | **JGE** |
| **Minore** (<) | **JB** *(Below)* | **JL** *(Less)* |
| **Minore o Uguale** (≤) | **JBE** | **JLE** |
</details><br>
<details>
<summary><h3>Loop</h3></summary>
  
```assembly
1:  ADD AX, (BX)(SI) 
		ADD SI,2 
		LOOP 1b
```
> Come vediamo, dopo aver sommato un elemento di realizza l'aggiornamento del Source Index e si invoca l'istruzione `LOOP`, essa oltre a saltare indietro all'etichetta `1` decrementa anche il registro `CX` nel quale è in genere conservato il contatore per le iterazioni, in questo caso in `CX` sarà conservata la lunghezza del vettore per continuare ad iterare fin quando vi sono elementi del vettore.
</details><br>
<details>
<summary><h3>Subroutine</h3></summary>

  
> Subroutine == Funzioni / Metodi
- Ricevere parametri in ingresso
- Utilizzare variabili locali
- Ritornare un valore

<details>
<summary><h3>Subroutine base</h3></summary>

```assembly
	   <...>
		 CALL Func
		 MOV SP,BP
		 <..>

Func:
		 PUSH BP
		 MOV BP,SP
		 <...>
		 MOV SP,BP
		 POP BP
		 RET		 
```
</details>
<details>
<summary><h3>Subroutine con argomenti</h3></summary>

```assembly
	   <...>
		 PUSH Arg3 ! Carico prima l'ultimo elemento
		 PUSH Arg2
		 PUSH Arg1 ! Carico per ultimo il primo elemento
		 CALL Func 
		 MOV SP,BP ! Reset dello stack
		 <..>

Func:
		 PUSH BP
		 MOV BP,SP
		 <...>
		 MOV AX, 4(BP) ! Primo elemento nello stack == Arg1
		 MOV BX, 6(BP) ! Secondo elemento nello stack == Arg2
		 MOV DX, 8(BP) ! Terzo elemento nello stack == Arg3
		 <...>
		 MOV SP,BP
		 POP BP
		 RET
```
</details>

</details><br>
<details>
<summary><h3>Operazioni su array e stringhe</h3></summary>

>Tutte le operazioni sono seguite dalla lettera `B`
>
>Esse sono spesso precedute e seguite dalle istruzioni `CLD` ed `STD`, che permettono di incrementare e decrementare gli indici ai puntatori utilizzati per scorrere una stringa; queste istruzioni lavorano quindi il flag delle direzioni `DF`.


<details>
<summary>MOVSB</summary>

> E’ `automatica`.
>
> Spostare `1Byte` dal registro `SI` alla posizione indirizzata dal registro `DI`.
> Usando `MOVSW` potremmo spostare una word `(2B)`, invece di un singolo Byte.
</details>
<details>
<summary>SCASB</summary>

>`Automatico`, ma necessariamente avere i due Byte in `DI` ed `AL`.
CMP `1Byte` indirizzato dal registro `DI` con un Byte contenuto nel registro `AL`.
Il registro `AL` non verrà alterato, `SCASW` potremmo confrontare una word `(2B)`.
</details>
<details>
<summary>CMPSB - Meglio evitare</summary>

>`Automatico`, necessariamente avere i due Byte in `SI` ed `DI`.
CMP `1Byte` indirizzato dal registro `SI` con un Byte contenuto nel registro `DI`.
`SI` e `DI` vengono INC o DEC in base al valore del bit di direzione del registro flag, `CMPSW` potremmo confrontare una word`(2B)`, invece di un singolo Byte.
</details>
<details>
<summary>LODSB - Meglio evitare</summary>

>`Automatico`. 
Trasferire un Byte indirizzato dal registro `DI` nel registro `AL`.
Need `2Byte` in `DI` (dato da trasferire)ed `AL`(destinazione in cui andare).
`DI` verrà INC o DEC a seconda del bit di direzione(`DF`) nel registro di flag.
`LODSB` potremmo trasferire una word, invece di un singolo Byte.
</details>
<details>
<summary>STOSB - Meglio evitare</summary>

>`Automatico`. 
Trasferire un Byte indirizzato dal registro `AL` nel registro `DI`.
Need `2Byte` in `AL` (dato da trasferire)ed `DI`(destinazione in cui andare).
`DI` verrà INC o DEC a seconda del bit di direzione(`DF`) nel registro di flag. `?`
`STOSB` potremmo trasferire una word, invece di un singolo Byte.
</details>

</details><br>
<details>
<summary><h3> Operazioni di ripetizione </h3></summary>

<details>
<summary>REP</summary>

> Ripetere una specifica istruzione da cui è seguita.

```assembly
REP SCASB
! Ripete SCASB CX volte
```
</details>

<details>
<summary>REPE o REPZ</summary>

> La REPE ripete l'istruzione fino a fine stringa, ovvero quando il registro CX=0.
</details>

<details>
<summary>REPNE o REPNZ</summary>

> Ripete fino a fine stringa, oppure fino a quando il confronto ha successo.
</details>
<!--
<details>
<summary>Collage REP</summary>
</details>
-->
</details><br>

</details>

***

<details>
<summary><h2> .SECT .DATA</h2></summary>

|Type|Size|
|:--|:--|
|.WORD|occupa 2 Byte|
|.BYTE |occupa 1 Byte|
|.ASCII |occupa 1 Byte|

```assembly
A:   .WORD 3
vec: .WORD 3,4,5,6,7
B:   .SPACE 1
C:   .ASCII "Hello"
```
</details>
