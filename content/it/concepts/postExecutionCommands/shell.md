---
title: Shell
weight: 1
cascade:
  type: docs
---

Il comando di controllo post esecuzione `shell` consente di eseguire comandi da terminale dopo l'esecuzione di un **test** per fare asserzioni di qualsiasi tipo, confrontando gli **exit code** dei comandi eseguiti.   
Il funzionamento è semplice, lo sviluppatore specifica il comando da eseguire da terminale ed eventualmente l'**exit code** atteso. Se l'**exit code** del comando eseguito non corrisponde a quello atteso, il comando di controllo post esecuzione fallisce e di conseguenza fallisce anche il controllo.   
L'**exit code** atteso è opzionale, se non specificato il valore di default è `0`.

## Esempio
```yaml
postExecChecks:
  - name: "Shell command check"
    type: "shell"
    args:
      cmd: "echo 'Hello'"
      expectedExitCode: 0 # Opzionale, il valore di default è 0
```