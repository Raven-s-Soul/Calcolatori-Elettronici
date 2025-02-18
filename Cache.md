# Cache

<details>
<summary>Seconda Prova parziale 2019</summary><br>

> Esercizio su cache. Si vuole progettare una cache a mappatura diretta per un sistema a 22 bit e blocchi di 32 byte. Rispondere alle domande che seguono supponendo di avere a disposizione uno spazio di 100KB sul microprocessore.

 ![Disegnino](/imgs/Cache.svg)

<details> 
<summary>Domande:</summary><br>

>a) Indicare la dimensione minima della cache in grado di garantire che su ciascuna slot non
collidano più di 128 blocchi. Esprimere il risultato nel formato XM (dove M=B,KB,MB,GB o
TB) arrotondando all'intero superiore (per esempio: 125KB).
<details>
<summary>Soluzione</summary><br>

>33Byte*2^(10) = 33KB dove 2^10 è la Line
</details>

>b) Indicare la struttura dell'indirizzo per la cache individuata nella domanda precedente,
specificando la dimensione dei vari campi (tag,line,byte) nel formato X:Y:Z (per esempio:
5:12:8).

<details>
<summary>Soluzione</summary><br>

>7:10:5
</details>

>c) Indicare la struttura di una slot della cache individuata nella domanda precedente,
specificando la dimensione dei vari campi (validità,tag,dati) nel formato XM:YM:ZM, dove
M = b (bit) o B (Byte) (per esempio: 3b:15b:55B).

<details>
<summary>Soluzione</summary><br>

>1b:7b:32B

</details>

>d) Indicare, indipendentemente dal numero di collisioni, la struttura dell'indirizzo che
consente di massimizzare la dimensione della cache specificando la dimensione dei vari
campi (tag,line,byte) nel formato X:Y:Z (per esempio: 5:12:8).

<details>
<summary>Soluzione</summary><br>

>6:11:5
>
>2^11(Line) * 33Byte = 64KB
</details>

>e) Indicare il numero di collisioni nel caso in cui, mantenendo lo stesso numero di slot della
cache individuata nelle domande precedenti, la cache viene realizzata a due vie.

<details>
<summary>Soluzione</summary><br>

>E’ equivalente a mettere 2 Blocchi uno dopo l’altro.
>
>???
</details>
</details>
</details>
