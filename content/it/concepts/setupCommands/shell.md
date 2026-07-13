---
title: Shell
weight: 1
cascade:
  type: docs
---

Il comando di setup di tipo *shell* consente di eseguire un qualsiasi comando da terminale prima e dopo l'esecuzione di un test, come per esempio avviare uno script *Python* per popolare un database.
Se si definisce un comando di setup di tipo *shell*, è obbligatorio specificarne uno dello stesso tipo per il ripristino al termine del test; quest'ultimo prende il nome di comando di **cleanup**.

#### Esempio
```yaml
setupCommands:
  - type: "shell"
    cmd: "docker compose -f docker-compose.yaml up -d"
    cleanupCmd:
      cmd: "docker compose -f docker-compose.yaml down -v"
  - cmd: "python3 setup.py" # type: "shell" è implicito
    cleanupCmd:
      cmd: "python3 cleanup.py"
```