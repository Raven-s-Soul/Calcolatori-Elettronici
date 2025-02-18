## Calcolatori Eletronici
> [!NOTE]
> 1. Il materiale è nel formato utile per **excalidraw** ovvero in formato `.excalidraw`
> 2. Apri [Excalidraw](https://excalidraw.com/) sul web e trascinaci il file ***scaricato***.

*Hw 5 - Esempio di processore*
- CPU.excalidraw

*Hw 6 - Assembly*
> Scrivere un programma in linguaggio assemblativo 8088 che, dato una stringa s (un vettore di caratteri) memorizzata in memoria principale:
>- stampa la lunghezza della stringa mediante una subroutine LUN che ha come argomenti l'indirizzo del primo carattere della stringa;
>- stampa il numero di volte che nella stringa compare un carattere x memorizzato in memoria principale mediante una subroutine OCC che ha come argomenti l'indirizzo del primo carattere della stringa e il carattere x.
>- Verificare il corretto funzionamento del programma con un assemblatore e consegnare in un unico file di formato a piacere
```Assembly
    _EXIT = 1
    _PRINTF = 127
.SECT .TEXT
start:
	PUSH S
	MOV BX, X
	CALL LUN
	MOV SP, BP
	PUSH BX
	PUSH A
	PUSH _PRINTF
	SYS
	MOV SP, BP
	PUSH (X)
	PUSH S
	CALL OCC
	MOV SP, BP
	PUSH DX
	PUSH B
	PUSH _PRINTF
	SYS
	MOV SP, BP
	PUSH 0
	PUSH _EXIT
	SYS

LUN:
	PUSH BP
	MOV BP, SP
	MOV AX,4(BP)
	SUB BX, AX ! len
	MOV SP,BP
	POP BP
	RET

OCC:
	PUSH BP
	MOV BP, SP
	MOV DI, 4(BP)  ! S
	MOVB AL, 6(BP)  ! X
	MOV CX, BX
	MOV DX,0 ! Counter
1:  SCASB
	JNE 2f
	ADD DX ,1
2:  LOOP 1b
	MOV SP,BP
	POP BP
	RET

.SECT .DATA
	S: .ASCII "aStringa"
	X: .ASCII "a"
	A: .ASCII "Lunghezza %d\n"
	C: .SPACE 1 ! needed string print error
	B: .ASCII "Occorrenza %d\n"
.SECT .BSS
```

### [Esonero logisim](/EsoneroLogisim.md)
