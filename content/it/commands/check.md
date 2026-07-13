---
title: Controllo sintattico
weight: 15
cascade:
  type: docs
---

Il comando `mtracer check` consente di eseguire un controllo sintattico di uno o più file di test con estensione `.mt.yaml`. Il comando verifica la correttezza della sintassi dei file e segnala eventuali errori.

## Esecuzione del comando
```bash
mtracer check [test1.mt.yaml] [test2] ... [flags]
```
È possibile specificare i test con o senza estensione `.mt.yaml`; in entrambi i casi, **Mtracer** riconoscerà correttamente il file.

Inoltre, se non viene specificato alcun file di test, il comando eseguirà il controllo sintattico su tutti i file con estensione `.mt.yaml` presenti nella cartella di lavoro corrente e nelle sue sottocartelle.

## Flags
Il comando `mtracer check` supporta i seguenti flag:
- `--dir` o `-d`: consente di specificare la cartella in cui eseguire il controllo sintattico dei file di test. Se non specificata, la cartella di lavoro sarà quella in cui si esegue il comando.
