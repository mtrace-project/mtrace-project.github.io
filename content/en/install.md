---
title: Installation
weight: 1
next: "/configure"
cascade:
  type: docs
---

**Mtracer** is available on `macOS`, `Windows` and `Linux`, and several installation methods are available for each operating system.

{{< tabs >}}
  {{< tab name="macOS" >}}
## Homebrew
```bash
brew install mtracer-project/tap/mtracer
xattr -d com.apple.quarantine $(which mtracer)
```
It is important to run the `xattr` command to remove the quarantine attribute, otherwise *macOS* may block the execution of **mtracer**.

## Terminal
Latest version installation:
```bash
curl -sL https://raw.githubusercontent.com/mtracer-project/mtracer/main/install.sh | bash
```
Specific version installation:
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

## Terminal
Latest version installation:
```bash
curl -sL https://raw.githubusercontent.com/mtracer-project/mtracer/main/install.sh | bash
```
Specific version installation:
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
