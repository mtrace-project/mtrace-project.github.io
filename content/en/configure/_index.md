---
title: Configuration
weight: 5
prev: "/install"
next: "/concepts"
cascade:
  type: docs
---

In this section, we will explore how to configure **Mtrace** for your end-to-end tests.

## Configuration methods
**Mtrace** supports various configuration methods to meet the needs of every developer and testing environment. The supported methods are listed below, ordered by decreasing priority (from highest to lowest):
1. **Command-line flags**: you can pass specific configuration parameters directly from the command line when executing any command.
2. **Environment variables**: you can define environment variables to configure **Mtrace**, or specify them inside a `.env` file at the root of the project (in the latter case, they will be loaded automatically).
3. **YAML configuration file**: you can define a YAML configuration file and specify its path using the `--config` or `-c` flag. If no path is provided, **Mtrace** will look for a file named `mtrace.yaml` in the project root by default.

If none of these options are used, **Mtrace** will fall back to its default configuration values.    
Furthermore, if a parameter is defined using multiple methods, the value from the method with the highest priority will be used.

## Configuration parameters nomenclature
Configuration parameters follow a specific nomenclature depending on the configuration method used. Below are the naming conventions for each method:
- **Command-line flags**: parameters can be explored via the `mtrace --help` or `mtrace -h` command. Flags generally consist of words separated by hyphens (`-`) and can be specified in a long format (e.g., `--config`) or a short format (e.g., `-c`).
- **Environment variables**: parameters can be specified as environment variables using uppercase letters and underscores (`_`) to separate words, for example, `MTRACE_BACKEND_TYPE`. Note that all environment variables must be prefixed with `MTRACE_` to be recognized by the application.
- **YAML configuration file**: parameters can be defined using the classic `key: value` syntax, such as `backend_type: jaeger`.   

It's also important to understand how YAML parameter names translate to environment variables. Below is a clarifying example:

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
MTRACE_BACKEND_TYPE=openobserve
MTRACE_OPENOBSERVE_BASE_URL=http://localhost:5080
```
  {{< /tab >}}
{{< /tabs >}}

In the documentation below, to refer to a YAML configuration variable, the `struct.variable_name` syntax will be used, indicating a YAML configuration structured like this:
```yaml
struct:
    variable_name: value
```

## Generic configuration parameters
The command-line flags defined in this section are available for all **Mtrace** commands.

| Environment variable   | YAML variable   | Command-line flag        | Description                                                                  | Default value     |
| ---------------------- | --------------- | ------------------------ | ---------------------------------------------------------------------------- | ----------------- |
| `MTRACE_BACKEND_TYPE`  | `backend_type`  |                          | Type of observability backend to use                                         | `openobserve`     |
|                        |                 | `--config`               | Path to the configuration file relative to the command execution directory   |                   |
| `MTRACE_DEBUG_ENABLED` | `debug_enabled` | `--debug`                | Enables debug mode                                                           | `false`           |
| `MTRACE_DIRECTORY`     | `directory`     | `--dir`                  | Working directory                                                            |                   |

## Observability backend configuration
The following sections detail the required configurations depending on the used backend. If a backend is not listed below, it means it is not currently supported by **Mtrace**.

{{% callout type="warning" %}}
**Mtrace** only supports trace analysis in the **OTel** format.
{{% /callout %}}
