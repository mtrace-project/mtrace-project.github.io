---
title: NATS and JetStream
cascade:
  type: docs
---

## Configuration
The NATS trigger supports both the NATS client and JetStream. In fact, the trigger supports the `nats` and `jetstream` types. The configuration is the same for both types, only the type of client used internally by **Mtracer** to connect to NATS changes.

The NATS trigger is configurable using the following arguments:
| Argument        | Description                            | Optional |
| --------------- | -------------------------------------- | -------- |
| `serverAddress` | NATS server address or hostname        | No       |
| `subject`       | Subject to publish the message to      | No       |
| `authType`      | Authentication type                    | No       |
| `headers`       | Headers to include in the message      | Yes      |
| `data`          | Message to publish                     | Yes      |
| `caPemPath`     | Path to the PEM file for TLS           | Yes      |

### Headers
The `headers` field specifies the headers to include in the message published to NATS, which must be specified as key-value pairs within a YAML map.

Example:
```yaml
headers:
    key1: "value1"
    key2: "value2"
```

### Data
The `data` field specifies the message to be published to NATS, which can be specified as preferred, knowing that before sending it will be converted to a **byte array**.

### Auth type
The `authType` field specifies the authentication type to use to connect to the NATS server.   
Supported authentication types are `noauth`, `userpass`, `token`, `jwt`, `nkey` and `mtls`.

#### No auth
If you use `noauth` as the authentication type, you do not need to specify any additional fields for authentication.

Example:
```yaml
authType: "noauth"
```


#### User and password
If you use `userpass` as the authentication type, you must specify the username and password through the `username` and `password` fields.

Example:
```yaml
authType: "userpass"
username: "admin"
password: "admin"
```

#### Token
If you use `token` as the authentication type, you must specify the token through the `token` field.

Example:
```yaml
authType: "token"
token: "mytoken"
```

#### JWT
If you use `jwt` as the authentication type, you must specify the JWT and the seed through the `jwt` and `seed` fields.

Example:
```yaml
authType: "jwt"
jwt: "myjwt"
seed: "myseed"
```

#### Nkey
If you use `nkey` as the authentication type, you must specify the seed through the `seed` field.

Example:
```yaml
authType: "nkey"
seed: "myseed"
```

#### mTLS
If you use `mtls` as the authentication type, you must specify the client certificate and the private key through the `clientCertPath` and `clientKeyPath` fields.

Example:
```yaml
authType: "mtls"
clientCertPath: "./client-cert.pem"
clientKeyPath: "./client-key.pem"
```
