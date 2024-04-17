# Scope ottimizzazione

## Tipi di scope

Abbiamo:
- **Ottimizzazione locale** $\longrightarrow$ lavora a livello dei BB, ad esempio abbiamo constand folding, propagazione delle costanti e dead-code elimination
- **Ottimizzazione globale** $\longrightarrow$ operano a livello del programma nel suo complesso, considerando il control flow graph e le dipendenze. Può coinvolgere più funzioni e moduli ed abbiamo inlining delle funzioni, DCE, flow anaylsis.
- **Ottimizzazione interprocedurale** $\longrightarrow$ di


## Dead code elimination

Iniziamo parlando delle ottimizzazioni locali esaminando la DC che sono delle istruzioni che definiscono una variabile non utilizzata. Ad esempio:
```cpp
main {
    int a = 4;
    int b = 2;
    int c = 1;
    int d = a + b;
    print(d)
    //Abbiamo quindi che "c" non è mai utilizzata e la riga può essere rimossa.
}
```

Anche la riga ```print(d)``` potrebbe rientrare nella definizione, il punto è che non è priva di **side effect** e, di conseguenza, non è dead code.

### Algoritmo

1. Per ogni istruzione nel BB:
    - Aggiungi i suoi operandi ad un metadato array **used**

2. Per ogni istruzione nel BB:
    - Se l'istruzione non ha una destinazione e non ha side effects, rimuovi l'istruzione
    - Altrimenti se la destinazione non corrisponde a nessuno degli elementi nell'array used rimuovi l'istruzione

Codice:
```cpp
    used =  {}
    for instr in BB:
        used += instr.args;
    
    for instr in BB:
        if instr.dest && instr.dest not in used:
            delete instr

```

Il problema principale qui è che ci sono istruzioni che potrebbero non venir rimosse come:
```cpp
main {
    int a = 4;
    int b = 2;
    int c = 1;
    int d = c + a;
    print(a)
    //Abbiamo quindi che "d" non è mai utilizzata e la riga può essere rimossa, ma se rimossa anche c andrebbe rimossa
}
```
Questo si risolve semplicemente facendo iterare il programma fino a quando non ci sono più modifiche:
```cpp
while program modified:
    used =  {}
    for instr in BB:
        used += instr.args;
    
    for instr in BB:
        if instr.dest && instr.dest not in used:
            delete instr

```