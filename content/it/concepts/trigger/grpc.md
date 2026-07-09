---
title: gRPC
cascade:
  type: docs
---

## Configurazione
Il trigger gRPC è configurabile attraverso il type `grpc` e i seguenti argomenti:
| Argomento          | Descrizione                               | Opzionale |
| ------------------ | ----------------------------------------- | --------- |
| `serverAddress`    | Indirizzo o hostname del server gRPC      | No        |
| `descriptorSource` | Fonte da cui ottenere il descriptor gRPC  | No        |
| `method`           | Nome del metodo gRPC da chiamare          | No        |
| `metadata`         | Metadata da includere nella chiamata gRPC | Si        |
| `data`             | Corpo della richiesta gRPC                | Si        |

### Descriptor source
Il campo `descriptorSource` specifica la fonte da cui ottenere il descriptor gRPC, che è necessario per poter effettuare la chiamata gRPC.
**Mtrace** supporta `file` e `serverReflection` come fonti per il descriptor gRPC, specificabili attraverso il campo `type` all'interno di `descriptorSource`.

#### File
Se si utilizza `file` come fonte per il descriptor gRPC, è necessario specificare il percorso del file del descriptor gRPC attraverso il campo `protoPath` all'interno di `descriptorSource`.

Esempio:
```yaml
descriptorSource:
    type: "file"
    protoPath: "./greeter.proto"
```

#### Server reflection
Se si utilizza `serverReflection` come fonte per il descriptor gRPC, è semplicemente necessario il tipo `serverReflection` all'interno di `descriptorSource`, in questo modo **Mtrace** utilizzerà la funzionalità di server reflection per ottenere il descriptor gRPC direttamente dal server gRPC specificato nell'argomento `serverAddress`.

Esempio:
```yaml
descriptorSource:
    type: "serverReflection"
```

### Method
Il campo `method` specifica il nome del metodo gRPC da chiamare, che deve essere specificato nel formato `package.service.method`, ad esempio `helloworld.Greeter.SayHello`.

### Metadata
Il campo `metadata` specifica i metadata da includere nella chiamata gRPC, che devono essere specificate come coppie chiave-valore all'interno di una mappa YAML.

Esempio:
```yaml
metadata:
    key1: "value1"
    key2: "value2"
```

### Data
Il campo `data` specifica il corpo della richiesta gRPC, che deve essere specificato in formato coppie chiave-valore all'interno di una mappa YAML, in questo modo **Mtrace** convertirà automaticamente la mappa YAML in un messaggio gRPC secondo il descriptor in possesso.

Esempio partendo da un messaggio gRPC definito come segue:
```protobuf
message RollRequest {
    string                    rollerName   = 1;
    google.protobuf.Timestamp rollTime     = 2;
    google.protobuf.Duration  rollDuration = 3;
}
```
Si converte in un corpo della richiesta gRPC specificato in YAML come segue:
```yaml
data:
    rollerName: "Alice"
    rollTime: "2023-10-01T12:00:00Z"
    rollDuration: "10s"
```