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

<details>
<summary><h3>.SECT .TEXT</h3></summary>



</details>
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
