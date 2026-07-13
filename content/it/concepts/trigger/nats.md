---
title: NATS e JetStream
cascade:
  type: docs
---

## Configurazione
Il trigger NATS supporta sia il client NATS che JetStream. Infatti il trigger supporta i type `nats` e `jetstream`. La configurazione è la stessa per entrambi i type, cambia solamente il tipo di client utilizzato internamente da **Mtracer** per connettersi a NATS.

Il trigger NATS è configurabile attraverso i seguenti argomenti:
| Argomento       | Descrizione                            | Opzionale |
| --------------- | -------------------------------------- | --------- |
| `serverAddress` | Indirizzo o hostname del server NATS   | No        |
| `subject`       | Subject su cui pubblicare il messaggio | No        |
| `authType`      | Tipo di autenticazione                 | No        |
| `headers`       | Headers da includere nel messaggio     | Si        |
| `data`          | Messaggio da pubblicare                | Si        |
| `caPemPath`     | Percorso del file PEM per TLS          | Si        |

### Headers
Il campo `headers` specifica gli headers da includere nel messaggio pubblicato su NATS, che devono essere specificati come coppie chiave-valore all'interno di una mappa YAML.

Esempio:
```yaml
headers:
    key1: "value1"
    key2: "value2"
```

### Data
Il campo `data` specifica il messaggio da pubblicare su NATS, il quale può essere specificato come si preferisce, sapendo che prima dell'invio verrà convertito come **array di byte**.

### Auth type
Il campo `authType` specifica il tipo di autenticazione da utilizzare per connettersi al server NATS.   
I tipi di autenticazione supportati sono `noauth`, `userpass`, `token`, `jwt`, `nkey` e `mtls`.

#### No auth
Se si utilizza `noauth` come tipo di autenticazione, non è necessario specificare ulteriori campi per l'autenticazione.

Esempio:
```yaml
authType: "noauth"
```


#### User e password
Se si utilizza `userpass` come tipo di autenticazione, è necessario specificare la username e la password attraverso i campi `username` e `password`.

Esempio:
```yaml
authType: "userpass"
username: "admin"
password: "admin"
```

#### Token
Se si utilizza `token` come tipo di autenticazione, è necessario specificare il token attraverso il campo `token`.

Esempio:
```yaml
authType: "token"
token: "mytoken"
```

#### JWT
Se si utilizza `jwt` come tipo di autenticazione, è necessario specificare il JWT e il seed attraverso i campi `jwt` e `seed`.

Esempio:
```yaml
authType: "jwt"
jwt: "myjwt"
seed: "myseed"
```

#### Nkey
Se si utilizza `nkey` come tipo di autenticazione, è necessario specificare il seed attraverso il campo `seed`.

Esempio:
```yaml
authType: "nkey"
seed: "myseed"
```

#### mTLS
Se si utilizza `mtls` come tipo di autenticazione, è necessario specificare il certificato client e la chiave privata attraverso i campi `clientCertPath` e `clientKeyPath`.

Esempio:
```yaml
authType: "mtls"
clientCertPath: "./client-cert.pem"
clientKeyPath: "./client-key.pem"
```