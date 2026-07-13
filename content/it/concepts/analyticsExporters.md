---
title: Esportazione delle statistiche
weight: 40
cascade:
  type: docs
---

**Mtracer** consente di esportare le statistiche delle *trace* e delle *span* generate dai test eseguiti.

Più metodi di esportazione possono essere specificati contemporaneamente ripetendo la flag `--analytics` o `-a` con valori diversi. I metodi supportati sono:
- `json`: esporta i dati delle statistiche in formato JSON.
- `html`: genera un report HTML interattivo con i dettagli delle statistiche.

Questa funzionalità diventa particolarmente utile eseguendo più volte lo stesso test tramite la flag `--count`, poiché consente di ottenere una panoramica completa delle prestazioni del sistema sotto test, evidenziando eventuali variazioni o anomalie nei risultati.

## Esportazione in formato JSON
Quando si utilizza il metodo di esportazione `json`, **Mtracer** genera un file JSON contenente le statistiche delle *trace* e delle *span* raccolte durante l'esecuzione dei test. Il file JSON può essere facilmente analizzato e integrato in strumenti di monitoraggio o pipeline di analisi dei dati.

Il file JSON contiene i seguenti campi:
| Campo                                                           | Descrizione                                                                                                                             |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `testName`                                                      | Nome della suite di test eseguita                                                                                                       |
| `timestamp`                                                     | Timestamp dell'esecuzione del test                                                                                                      |
| `traces`                                                        | Array contenente i dati delle *trace* raccolte durante l'esecuzione del test                                                            |
| `traceAnalytics`                                                | Oggetto contenente le statistiche globali delle *trace* raccolte durante l'esecuzione del test                                          |
| `traceAnalytics.minDuration`                                    | Durata minima tra tutte le *trace* raccolte (in millisecondi)                                                                           |
| `traceAnalytics.maxDuration`                                    | Durata massima tra tutte le *trace* raccolte (in millisecondi)                                                                          |
| `traceAnalytics.p50Duration`                                    | 50° percentile (mediana) della durata delle *trace* (in millisecondi)                                                                   |
| `traceAnalytics.p90Duration`                                    | 90° percentile della durata delle *trace* (in millisecondi)                                                                             |
| `traceAnalytics.p99Duration`                                    | 99° percentile della durata delle *trace* (in millisecondi)                                                                             |
| `traceAnalytics.durationStandardDeviation`                      | Deviazione standard della durata delle *trace*                                                                                          |
| `traceAnalytics.averageDuration`                                | Durata media delle *trace* raccolte                                                                                                     |
| `traceAnalytics.averageSpanCount`                               | Numero medio di *span* per ogni *trace*                                                                                                 |
| `traceAnalytics.averageSpanErrorCount`                          | Numero medio di *span* con errore per ogni *trace*                                                                                      |
| `traceAnalytics.errorRate`                                      | Percentuale di *trace* che contengono almeno uno *span* con stato di errore                                                             |
| `traceAnalytics.spanAnalytics`                                  | Array contenente le statistiche dettagliate raggruppate per *span* di diverse *trace* con lo stesso **serviceName** e **operationName** |
| `traceAnalytics.spanAnalytics.[].serviceName`                   | Nome del servizio a cui appartiene lo *span*                                                                                            |
| `traceAnalytics.spanAnalytics.[].operationName`                 | Nome dell'operazione eseguita dallo *span*                                                                                              |
| `traceAnalytics.spanAnalytics.[].minDuration`                   | Durata minima registrata per questa operazione (in millisecondi)                                                                        |
| `traceAnalytics.spanAnalytics.[].maxDuration`                   | Durata massima registrata per questa operazione (in millisecondi)                                                                       |
| `traceAnalytics.spanAnalytics.[].p50Duration`                   | 50° percentile (mediana) della durata di questa operazione (in millisecondi)                                                            |
| `traceAnalytics.spanAnalytics.[].p90Duration`                   | 90° percentile della durata di questa operazione (in millisecondi)                                                                      |
| `traceAnalytics.spanAnalytics.[].p99Duration`                   | 99° percentile della durata di questa operazione (in millisecondi)                                                                      |
| `traceAnalytics.spanAnalytics.[].durationStandardDeviation`     | Deviazione standard della durata di questa operazione                                                                                   |
| `traceAnalytics.spanAnalytics.[].averageDuration`               | Durata media di questa operazione                                                                                                       |
| `traceAnalytics.spanAnalytics.[].errorRate`                     | Tasso di errore specifico per questa operazione (in percentuale)                                                                        |
| `traceAnalytics.spanAnalytics.[].averagePercentageOfTotalTrace` | Percentuale media del tempo impiegato da questo *span* rispetto alla durata totale della *trace* padre                                  |

{{% callout type="info" %}}
Gli *span* spesso si sovrappongono a livello temporale, essendo per la maggior parte chiamate sincrone. Pertanto, è necessario prestare attenzione alla metrica `averagePercentageOfTotalTrace`, dato che la somma di tutte le percentuali degli *span* molto probabilmente non sarà pari al 100%.
{{% /callout %}}


### Esportazione in formato HTML
Quando si utilizza il metodo di esportazione `html`, **Mtracer** genera un report HTML interattivo che consente di visualizzare le statistiche delle *trace* e delle *span* in modo più intuitivo. Il report HTML include grafici e tabelle che facilitano l'analisi delle prestazioni del sistema sotto test, permettendo di identificare rapidamente eventuali colli di bottiglia o anomalie nei risultati.

Qui sotto è possibile visualizzare delle immagini di esempio del report HTML generato da **Mtracer**:
![Esempio di report HTML - Panoramica delle trace](/images/analytics-export-html-1.png)
![Esempio di report HTML - Panoramica delle trace](/images/analytics-export-html-2.png)

Il report HTML mostra:
- le principali statistiche delle *trace* e delle *span* raccolte durante l'esecuzione dei test;
- il grafico delle durate della *trace* suddivise per numero di esecuzione;
- due istogrammi che paragonano le diverse metriche delle durate e il numero di *span* medio comparato al numero medio di *span* con errore;
- una tabella dettagliata delle statistiche delle *span* raggruppate per **serviceName** e **operationName**;
- le *trace* e le *span* effettivamente raccolte per ogni run del test raggruppate per **traceId**. Mostrano ogni *span* contenuta nella *trace* selezionata.

Inoltre, dal menù a tendina in alto a sinistra è possibile selezionare il test specifico per cui si vogliono visualizzare le statistiche.