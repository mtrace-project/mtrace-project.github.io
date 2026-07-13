---
title: Configuration
weight: 5
prev: "/install"
next: "/concepts"
cascade:
  type: docs
---

In this section, we will explore how to configure **Mtracer** for *end-to-end* testing.

## Configuration methods
**Mtracer** supports several configuration methods to meet the needs of every developer and every testing environment. The supported methods are listed below, in descending order of priority (from most to least relevant):
1. **Command line flags**: it is possible to pass some configuration parameters directly from the command line during execution;
2. **Environment variables**: it is possible to define environment variables directly in the system or within a `.env` file in the project root (in the latter case, they will be loaded automatically);
3. **YAML configuration file**: it is possible to define a configuration file in YAML format by specifying its path with the `--config` or `-c` flag. If no path is specified, **Mtracer** will look for a file named `mtracer.yaml` in the project root by default.

If none of these options are used, **Mtracer** will apply default values.    
Furthermore, if a parameter is specified in more than one configuration method, the value provided by the method with the highest priority will be used.

## Configuration parameter nomenclature
Configuration parameters follow a specific nomenclature depending on the method used. The conventions for each method are shown below:
- **Command line flags**: parameters can be explored via the `mtracer --help` or `mtracer -h` command. Flags generally consist of words separated by hyphens (`-`) and can be specified in long (e.g., `--config`) or short (e.g., `-c`) form.
- **Environment variables**: environment variables must be written in uppercase, with words separated by underscores (`_`), for example `MTRACER_BACKEND_TYPE`. It is important to note that all variables must have the `MTRACER_` prefix to be recognized.
- **YAML configuration file**: parameters are specified using standard `key: value` syntax (e.g., `backend_type: jaeger`).   

It is essential to understand how YAML names are converted into environment variables. An explanatory example is provided below:

{{< tabs >}}
  {{< tab name="YAML" >}}
```yaml
backend_type: openobserve
openobserve:
    base_url: http://localhost:5080
```
  {{< /tab >}}
  {{< tab name="Environment variables" >}}
  ```bash
MTRACER_BACKEND_TYPE=openobserve
MTRACER_OPENOBSERVE_BASE_URL=http://localhost:5080
```
  {{< /tab >}}
{{< /tabs >}}

In the subsequent documentation, to refer to a variable defined in YAML, the syntax `struct.variable_name` will be used, indicating a configuration of this type:
```yaml
struct:
    variable_name: value
```

## Generic configuration parameters
The command line flags described in this section are available for all **Mtracer** commands.

| Environment Variable   | YAML Variable  | Command Line Flag | Description                                                              | Default Value     |
| ---------------------- | -------------- | ----------------- | ------------------------------------------------------------------------ | ----------------- |
| `MTRACER_BACKEND_TYPE` | `backend_type` |                   | Type of observability backend to use                                     | `openobserve`     |
|                        |                | `--config`        | Path to the configuration file relative to the execution folder          |                   |
| `MTRACER_DIRECTORY`    | `directory`    | `--dir`           | Working directory in which to execute the command                        |                   |
| `MTRACER_QUIET`        | `quiet`        | `--quiet`         | Disables the output of the executed command result                       | `false`           |
| `MTRACER_VERBOSE`      | `verbose`      | `--verbose`       | Enables detailed output of the executed command                          | `false`           |

## Observability backend configuration
The following sections illustrate the necessary configurations depending on the backend used. If a backend is not on the list, it means that it is currently not supported by **Mtracer**.

{{% callout type="warning" %}}
**Mtracer** supports trace analysis only in **OTel** format.
{{% /callout %}}
