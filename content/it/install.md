---
title: Installazione
weight: 1
next: "/configure"
cascade:
  type: docs
---

**Mtracer** è disponibile su `macOS`, `Windows` e `Linux`, inoltre sono disponibili diversi metodi di installazione per ogni sistema operativo.

{{< tabs >}}
  {{< tab name="macOS" >}}
## Homebrew
```bash
brew install mtracer-project/tap/mtracer
xattr -d com.apple.quarantine $(which mtracer)
```
È importante eseguire il comando `xattr` per rimuovere l'attributo di quarantena, altrimenti *macOS* potrebbe bloccare l'esecuzione di **mtracer**.

## Terminale

Installazione dell'ultima versione:
```bash
curl -sL https://raw.githubusercontent.com/mtracer-project/mtracer/main/install.sh | bash
```

Installazione di una versione specifica:
```bash
curl -sL https://raw.githubusercontent.com/mtracer-project/mtracer/develop/install.sh | bash -s -- v0.2.0
```

## Go
```bash
go install github.com/mtracer-project/mtracer@latest
```

  {{< /tab >}}
  {{< tab name="Windows" >}}

## Scoop
```bash
scoop bucket add mtracer https://github.com/mtracer-project/scoop-bucket.git
scoop install mtracer
```

## Winget
```bash
winget install mtracer.mtracer
```

## Go
```bash
go install github.com/mtracer-project/mtracer@latest
```

  {{< /tab >}}
  {{< tab name="Linux" >}}

## Terminale

Installazione dell'ultima versione:
```bash
curl -sL https://raw.githubusercontent.com/mtracer-project/mtracer/main/install.sh | bash
```

Installazione di una versione specifica:
```bash
curl -sL https://raw.githubusercontent.com/mtracer-project/mtracer/develop/install.sh | bash -s -- v0.2.0
```

## Homebrew
```bash
brew install mtracer-project/tap/mtracer
```

## Go
```bash
go install github.com/mtracer-project/mtracer@latest
```

  {{< /tab >}}
{{< /tabs >}}
