# Guida per Assembly 101

### I file in Assembly estensione .s
```javascript
.SECT .TEXT == istruzioni 
.SECT .DATA == dati
.SECT .BSS  == dati non inizializzati 

! questo è un commento in Assembly
```
<details>
<summary><h3> Macros</h3></summary>

> This is the only macros you need to know.
```assembly
_EXIT = 1
_PRINTF = 127
```
Metti le macro prima di .SECT .TEXT
</details>

***

<details>
<summary><h3>.SECT .TEXT</h3></summary>
<br> 
<details>
<summary>Registri</summary>

- AX = Accumulatore
- SP & BP = Stack usage

![Registri](/imgs/1-min.png)
</details><br>
<details>
<summary>Access Types</summary>

|Notazioni | Spiegazione |
|:--:|:--|
|e| indirizzo effettivo|
|r|registro|
|#|operando immediato {Tipo un numero Dec, Hex, …}|

![Access Types](/imgs/2-min.png)
</details><br>
<details>
<summary>Funzioni Base</summary>

![Funzioni Base1](/imgs/3-min.png)
![Funzioni Base2](/imgs/4-min.png)
</details><br>
<details>
<summary>Jumps (JMP)</summary>
  
> Solo i jmp semplici
![Jumps](/imgs/5-min.png)
</details><br>
<details>
<summary>Loop</summary>
  
```assembly
1:  ADD AX, (BX)(SI) 
		ADD SI,2 
		LOOP 1b
```
> Come vediamo, dopo aver sommato un elemento di realizza l'aggiornamento del Source Index e si invoca l'istruzione `LOOP`, essa oltre a saltare indietro all'etichetta `1` decrementa anche il registro `CX` nel quale è in genere conservato il contatore per le iterazioni, in questo caso in `CX` sarà conservata la lunghezza del vettore per continuare ad iterare fin quando vi sono elementi del vettore.
</details><br>
<details>
<summary>Subroutine </summary>

  
> Subroutine == Funzioni / Metodi
- Ricevere parametri in ingresso
- Utilizzare variabili locali
- Ritornare un valore
<details>
<summary> Esempio grafico </summary>

![Jumps](/imgs/6-min.png)
</details>
<details>
<summary> Subroutine base </summary>

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
<summary> Subroutine con argomenti </summary>

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
<summary> Operazioni su array e stringhe </summary>

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
<summary> Operazioni di ripetizione </summary>

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
<summary><h3> .SECT .DATA</h3></summary>

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
