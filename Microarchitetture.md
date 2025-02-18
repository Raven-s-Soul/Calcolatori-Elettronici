# Microarchitettura CPU
> [!IMPORTANT]
>Tipologie di processori:
>- `RISC`
>- `CISC`

<details> 
<summary><h3>Esempio registri e bus</h3></summary><br>
  
> - Osserviamo un'architettura è a 32 con un bus parallelo a 32 bit.

![Microarchitettura](/imgs/Microarchitettura.png)
</details>

### Le cui componenti principali sono:

<details> 
<summary>I registri</summary><br>
  
  |ACRONIMO|NOME|Spiegazione|
  | :--: | :-- | :-- |
  |MAR|Memory Address Register|contenente indirizzi per caricare un dato dalla memoria principale|
  |MDR|Memory Data Register|per caricare i dati dalla memoria|
  |PC|Program Counter|contiene l'indirizzo della prossima istruzione da eseguire|
  |MBR|Memory Buffer Register|registro delle istruzioni|
    
> 💡 Si chiama MBR perchè stiamo implementando una JVM -> l'MBR è registro di 8 bit, piú piccolo degli altri registri, perchè il linguaggio Java Byte Code è realizzato con istruzioni a 8 bit

- SP
- LV
- CPP
- TOS
- OPC
- `H` → Holding → esso mantiene un contenuto in attesa che l'ALU lo utilizzi; in generale questo è chiamato in letteratura `accumulatore` e mantiene dei dati. Fu implementato per poter diminuire il numero di Bus, ed ottenerne due, e non tre. Avremo quindi un canale che consente di far entrare nell'ingresso dell'ALU il contenuto dell'accumulatore.
</details>

<details> 
<summary>Bus</summary><br>
  
- A
    - Esso permette di far  comunicare il contenuto del registro `accumulatore` all'ALU.
- B
    - Permette di far comunicare il contenuto di tutti i registri, eccetto H, con l'ALU.
- C
    - Restituisce il risultato delle operazioni dell'ALU e dello shifter e permette di salvarle.
</details>
<details> 
<summary>ALU</summary><br>
    
> `n` bit di controllo = $`2^n -1`$ operazioni, poichè esiste in non fare nulla.
</details>
<details> 
<summary>Shifter</summary><br>
    
> Si è pensato di utilizzare un shifter subito dopo l'ALU per permettere a quest'ultima di `effettuare operazioni più complesse di uno shift di un bit`.
</details>
</details>

> [!TIP]
> Osserviamo che avremo bisogno non solo dei segnali di controllo ma di un segnale di temporizzazione che ci permetterà di scandire le operazioni da effettuare in dei cicli

## Segnali di temporizzazione

- Temporizzazione del ciclo base
- Accesso alla memoria
- Che architettura useremo?
- Come sono fatte le micro istruzioni?
- La sezione di controllo
- Come possiamo pensare un'architettura RISC a partire da una CISC?
- Come migliorare le prestazioni di un microprocessore?
