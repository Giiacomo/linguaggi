# Lezione 3 - Lab 1 - Introduzione ai passi di LLVM

## Scrivere un passo LLVM

Abbiamo visto che il middle-end organizza la sua ottimizzazioni in **passi** che possono essere di:
- Analisi -> $\diamond$ da completare 
- Trasformazione -> alterano IR creando trasformazioni

Per ogni file che si vuole compilare esistono:
- **LLVM Module**   $\longrightarrow$ lista di functions
- **Functions**     $\longrightarrow$ lista di BB e argomenti
- **Basic Blocks**  $\longrightarrow$ lista di instruction
- **Instructions**  $\longrightarrow$ opcode e operandi

Si farà ciò tramite gli **Iteratori** e il **Downcasting** (permette di specificare il comportamento a cui siamo interessati).
