# Data Flow Analysis

Si contrappone alla scorsa lezione perchè è un'analisi globale, dove si controlla ogni basic block.
Ci permette di derivare da ogni variabile _x_ informazioni come:
- Valore di _x_
- Istruzione che definisce _x_
- Se la definizione è **live**

## Effetti BB

Un'istruzione può avere diversi effetti, tra cui, considerando ad esempio ```a = b  +  c```:
- **Use** $\longrightarrow$ delle variabili (b, c)
- **Kill** $\longrightarrow$ una precedente definizione (a)
- **Define** $\longrightarrow$ una variabile (a)

Componendo gli effetti delle singole istruzioni arriviamo agli effetti di un basic block:
- Un **uso localmente esposto** in un BB è un uso di una variabile che non è preceduto nel BB da una definizione della stessa variabile
- Ogni definizione di una var nel BB uccide tutte le definizioni della stessa variabile che sono in grado di arrivare a quel BB
- Una **definizione localmente disponibile** è l'ultima definizione di una variabile nel BB


Ad esempio: 
```c
t1 = r1 + r2 // Usi localmente esposti: r1, r2
r2 = t1 // Definizioni uccise: r2
t2 = r2 + r1 
r1 = t2
t3 = r1 * r1
r2 = t3
if r2 > 100 goto L1
```

## Reaching Definitions

Si può considerare ogni **istruzione di assegnamento** come una **definizione**. Una definizione _d_ raggiunge (**reaches**) un punto _p_ se **esiste** un percorso da _d_ a _p_ per cui _d_ **non è uccisa** lungo quel percorso.

Si creano quindi tanti **bit vector** quanti sono i punti del programma, ovvero le sue istruzioni e la lunghezza di questi vettori è pari al numero di definizioni.

Anche se ci sono branch in cui una definizione viene "killata", se esiste almeno un branch che la collega al punto, allora c'è reach.