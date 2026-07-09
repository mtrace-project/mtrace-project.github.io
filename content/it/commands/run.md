---
title: Esecuzione test
weight: 10
cascade:
  type: docs
---

Il comando `mtrace run` consente di eseguire i test passati come argomenti o, in assenza di questi, tutti i test presenti nella cartella di lavoro.   
I test vengono eseguiti sequenzialmente per evitare che le configurazioni di uno interferiscano con quelle degli altri.

## Esecuzione del comando
```bash
mtrace run [test1.mt.yaml] [test2] ... [flags]
```
È possibile specificare i test con o senza estensione `.mt.yaml`; in entrambi i casi, **Mtrace** riconoscerà correttamente il file.

Inoltre, se non viene specificato alcun test, verranno eseguiti tutti i test presenti nella cartella di lavoro corrente e nelle sue sottocartelle.

## Flags
Il comando `mtrace run` supporta i seguenti flag:
- `--dir` o `-d`: consente di specificare la cartella di lavoro. Se non specificata, la cartella di lavoro sarà quella in cui si esegue il comando.
- `--debug` o `-D`: abilita la modalità debug, mostrando informazioni più dettagliate durante l'esecuzione dei test.
