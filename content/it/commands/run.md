---
title: Esecuzione test
weight: 10
cascade:
  type: docs
---

Il comando `mtracer run` consente di eseguire i test passati come argomenti o, in assenza di questi, tutti i test presenti nella cartella di lavoro.   
I test vengono eseguiti sequenzialmente per evitare che le configurazioni di uno interferiscano con quelle degli altri.

## Esecuzione del comando
```bash
mtracer run [test1.mt.yaml] [test2] ... [flags]
```
È possibile specificare i test con o senza estensione `.mt.yaml`; in entrambi i casi, **Mtracer** riconoscerà correttamente il file.

Inoltre, se non viene specificato alcun test, verranno eseguiti tutti i test presenti nella cartella di lavoro corrente e nelle sue sottocartelle.

## Flags
Il comando `mtracer run` supporta i seguenti flag:
- `--count` o `-c`: consente di specificare il numero di volte in cui eseguire i test. Se non specificato, i test verranno eseguiti una sola volta.
- `--export-to` o `-e`: permette l'esportazione dei risultati dei test in diversi formati contemporaneamente, ripetendo la stessa flag con un valore diverso. I formati supportati sono: `json`, `markdown` o `md` e `junit`.    
Esempio: `mtracer run test1.mt.yaml -e json -e md` esporterà i risultati in formato JSON e Markdown.
- `--analytics` o `-a`: consente l'esportazione delle statistiche delle trace e delle span generate dai test eseguiti. Più formati di esportazione possono essere specificati contemporaneamente, ripetendo la stessa flag con un valore diverso. I formati supportati sono: `json` e `html`. È possibile approfondire la funzionalità nella pagina [Esportazione delle statistiche](/concepts/analyticsExporters).
- `--dir` o `-d`: consente di specificare la cartella di lavoro. Se non specificata, la cartella di lavoro sarà quella in cui si esegue il comando.
- `--verbose` o `-V`: abilita un output dettagliato con lo scopo di fornire informazioni più approfondite sull'esecuzione dei test.
- `--quiet` o `-q`: disabilita l'output del risultato dei test eseguiti, mostrando solo eventuali errori o avvertenze.

{{% callout type="warning" %}}
È importante sapere che i percorsi definiti all'interno dei test devono essere relativi alla posizione del file di test stesso, e non alla cartella di lavoro specificata con il flag `--dir` o in cui si esegue il comando.
{{% /callout %}}
