---
title: Configurazione
weight: 5
prev: "/install"
next: "/concepts"
cascade:
  type: docs
---

In questa sezione verrà esplorato come configurare **Mtracer** per i test *end-to-end*.

## Metodi di configurazione
**Mtracer** supporta diversi metodi di configurazione per venire incontro alle esigenze di ogni sviluppatore e di ogni ambiente di test. I metodi supportati sono elencati di seguito, in ordine decrescente di priorità (dal più al meno rilevante):
1. **Flag da riga di comando**: è possibile passare alcuni parametri di configurazione direttamente dalla riga di comando durante l'esecuzione;
2. **Variabili d'ambiente**: è possibile definire le variabili d'ambiente direttamente a sistema o all'interno di un file `.env` nella root del progetto (in quest'ultimo caso, verranno caricate automaticamente);
3. **File di configurazione YAML**: è possibile definire un file di configurazione in formato YAML specificandone il percorso con il flag `--config` o `-c`. Se non viene specificato alcun percorso, **Mtracer** cercherà di default un file denominato `mtracer.yaml` nella root del progetto.

Se non viene utilizzata nessuna di queste opzioni, **Mtracer** applicherà i valori di default.    
Inoltre, se un parametro è specificato in più di un metodo di configurazione, verrà utilizzato il valore fornito dal metodo con la priorità più alta.

## Nomenclatura dei parametri di configurazione
I parametri di configurazione seguono una specifica nomenclatura a seconda del metodo utilizzato. Di seguito sono riportate le convenzioni per ciascun metodo:
- **Flag da riga di comando**: i parametri possono essere esplorati tramite il comando `mtracer --help` o `mtracer -h`. I flag sono generalmente composti da parole separate da trattini (`-`) e possono essere specificati in forma lunga (es. `--config`) o breve (es. `-c`).
- **Variabili d'ambiente**: le variabili d'ambiente devono essere scritte in maiuscolo, con le parole separate da underscore (`_`), ad esempio `MTRACER_BACKEND_TYPE`. È importante notare che tutte le variabili devono avere il prefisso `MTRACER_` per essere riconosciute.
- **File di configurazione YAML**: i parametri vengono specificati con la sintassi standard `chiave: valore` (es. `backend_type: jaeger`).   

È fondamentale capire come i nomi YAML vengano convertiti in variabili d'ambiente. Di seguito è riportato un esempio esplicativo:

{{< tabs >}}
  {{< tab name="YAML" >}}
```yaml
backend_type: openobserve
openobserve:
    base_url: http://localhost:5080
```
  {{< /tab >}}
  {{< tab name="Variabili d'ambiente" >}}
  ```bash
MTRACER_BACKEND_TYPE=openobserve
MTRACER_OPENOBSERVE_BASE_URL=http://localhost:5080
```
  {{< /tab >}}
{{< /tabs >}}

Nella documentazione successiva, per fare riferimento a una variabile definita in YAML, verrà utilizzata la sintassi `struct.nome_variabile`, indicando una configurazione di questo tipo:
```yaml
struct:
    nome_variabile: valore
```

## Parametri di configurazione generici
I flag da riga di comando descritti in questa sezione sono disponibili per tutti i comandi di **Mtracer**.

| Variabile d'ambiente   | Variabile YAML | Flag da riga di comando | Descrizione                                                              | Valore di default |
| ---------------------- | -------------- | ----------------------- | ------------------------------------------------------------------------ | ----------------- |
| `MTRACER_BACKEND_TYPE` | `backend_type` |                         | Tipo di observability backend da utilizzare                              | `openobserve`     |
|                        |                | `--config`              | Percorso del file di configurazione relativo alla cartella di esecuzione |                   |
| `MTRACER_DIRECTORY`    | `directory`    | `--dir`                 | Cartella di lavoro in cui eseguire il comando                            |                   |
| `MTRACER_QUIET`        | `quiet`        | `--quiet`               | Disabilita l'output del risultato del comando eseguito                   | `false`           |
| `MTRACER_VERBOSE`      | `verbose`      | `--verbose`             | Abilita l'output dettagliato del comando eseguito                        | `false`           |

## Configurazione observability backend
Nelle sezioni successive sono illustrate le configurazioni necessarie a seconda del backend utilizzato. Se un backend non è presente nell'elenco, significa che attualmente non è supportato da **Mtracer**.

{{% callout type="warning" %}}
**Mtracer** supporta l'analisi delle trace solamente in formato **OTel**.
{{% /callout %}}