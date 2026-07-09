---
title: Creazione test
weight: 5
cascade:
  type: docs
---

Il comando `mtrace create` consente di creare un nuovo file di test con estensione `.mt.yaml` all'interno della cartella specificata (o in quella corrente, se non viene specificata). Il file di test creato conterrà una struttura di base che potrà essere modificata a seconda delle esigenze dello sviluppatore.

## Esecuzione del comando
```bash
mtrace create [nome_test] [flags]
```
L'estensione `.mt.yaml` verrà aggiunta automaticamente al nome del file, quindi non è necessario includerla nel comando. Ad esempio, se si esegue il comando `mtrace create test1`, verrà creato un file con nome `test1.mt.yaml`.

## Flags
Il comando `mtrace create` supporta i seguenti flag:
- `--dir` o `-d`: consente di specificare la cartella in cui creare il file di test. Se non specificata, il file verrà creato nella cartella in cui si esegue il comando.
- `--trigger-type` o `-t`: consente di specificare il tipo di *trigger* da utilizzare per l'avvio della *trace*. Se non viene specificato, verrà utilizzato **HTTP** come tipo di default.    
È possibile visualizzare tutti i tipi di trigger supportati consultando la sezione [Trigger supportati]({{< ref "../concepts/trigger.md#trigger-supportati" >}}).