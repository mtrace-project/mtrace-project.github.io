---
title: NATS and JetStream
cascade:
  type: docs
---

## Configuration
The NATS trigger supports both the NATS client and JetStream. Indeed, the trigger supports both `nats` and `jetstream` types. The configuration is the same for both types; the only difference is the type of client used internally by **Mtrace** to connect to NATS.

The NATS trigger is configurable using the following arguments:
| Argument        | Description                            | Optional  |
| --------------- | -------------------------------------- | --------- |
| `serverAddress` | NATS server address or hostname        | No        |
| `subject`       | Subject to publish the message to      | No        |
| `authType`      | Type of authentication                 | No        |
| `headers`       | Headers to include in the message      | Yes       |
| `data`          | Message to publish                     | Yes       |
| `caPemPath`     | Path to the PEM file for TLS           | Yes       |

### Headers
The `headers` field specifies the headers to include in the message published to NATS, which must be provided as key-value pairs within a YAML map.

Example:
```yaml
headers:
    key1: "value1"
    key2: "value2"
```

### Data
The `data` field specifies the message to publish to NATS, which can be defined in any format, keeping in mind that it will be converted to a **byte array** before being sent.

### Auth type
The `authType` field specifies the type of authentication to use to connect to the NATS server.   
The supported authentication types are `noauth`, `userpass`, `token`, `jwt`, `nkey`, and `mtls`.

#### No auth
If `noauth` is used as the authentication type, you do not need to specify any additional authentication fields.

Example:
```yaml
authType: "noauth"
```

#### User and password
If `userpass` is used as the authentication type, you must specify the username and password via the `username` and `password` fields.

Example:
```yaml
authType: "userpass"
username: "admin"
password: "admin"
```

#### Token
If `token` is used as the authentication type, you must specify the token via the `token` field.

Example:
```yaml
authType: "token"
token: "mytoken"
```

#### JWT
If `jwt` is used as the authentication type, you must specify the JWT and the seed via the `jwt` and `seed` fields.

Example:
```yaml
authType: "jwt"
jwt: "myjwt"
seed: "myseed"
```

#### Nkey
If `nkey` is used as the authentication type, you must specify the seed via the `seed` field.

Example:
```yaml
authType: "nkey"
seed: "myseed"
```

#### mTLS
If `mtls` is used as the authentication type, you must specify the client certificate and the private key via the `clientCertPath` and `clientKeyPath` fields.

Example:
```yaml
authType: "mtls"
clientCertPath: "./client-cert.pem"
clientKeyPath: "./client-key.pem"
```
