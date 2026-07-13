---
title: Docker
weight: 2
cascade:
  type: docs
---

The *docker* setup command allows you to perform specific operations on Docker containers, such as stopping or slowing down a container before a test execution.

This command type supports the operations described below.

## Container State Operations
These operations allow you to modify the state of a Docker container, for instance to simulate a service outage.

| Operation          | Description                                                                             |
| ------------------ | --------------------------------------------------------------------------------------- |
| `killContainer`    | Terminates the container execution without removing it by sending a **SIGKILL** signal  |
| `stopContainer`    | Stops the container execution without removing it by sending a **SIGTERM** signal       |
| `startContainer`   | Starts the container execution                                                          |
| `pauseContainer`   | Pauses the container execution                                                          |
| `unpauseContainer` | Resumes the container execution                                                         |

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "stopContainer"
    args:
      containerId: "container-id|container-name"
```
All of the aforementioned operations require a `containerId` argument, which can be either the ID or the name of the container to perform the operation on.

## Executing Commands Inside a Container
This operation allows you to run a command inside a Docker container, for example, to apply specific configurations prior to a test execution.
The operation is named `execContainer` and requires two arguments: `containerId` and `cmd` (the command to execute within the container).
It also requires a corresponding cleanup command of the same type, which runs another command inside the same container after the test has finished, for example, to revert the configurations modified by the setup command.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "execContainer"
    args:
      containerId: "container-id|container-name"
      cmd: "echo 'Setup complete!'"
    cleanupCmd:
      cmd: "echo 'Cleanup complete!'"
```

## Starting Containers with Docker Compose
This operation lets you spin up containers defined in a given **Docker Compose** configuration file and wait for all of them to be running or in a *healthy* state before proceeding with the test execution or the following setup commands.
The operation is called `composeUp` and requires a `composePath` argument representing the path to the **Docker Compose** file, along with an optional `projectName` argument to specify the Docker Compose project name. If the project name is not provided, the name of the directory containing the configuration file will be used.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "composeUp"
    args:
      composePath: "docker-compose.yaml"
      projectName: "my-project" # optional argument, defaults to the directory name
```

## Simulating Network Issues Inside a Container
These operations allow you to simulate network issues within a Docker container, such as introducing latency or limiting available bandwidth. Such operations require the `containerId` along with other specific arguments.

Furthermore, you can specify the virtual network interface to apply the operation to via the `netInterface` argument; otherwise, the default `eth0` interface will be used.
In most cases, a Docker container without custom network configurations defaults to the `eth0` interface; however, you can verify the interface name by running the `ip addr` command inside the container.

{{% callout type="info" %}}
**Mtracer** bases this functionality on the use of [`tc`](https://man7.org/linux/man-pages/man8/tc.8.html), a Linux utility for configuring the network subsystem. This tool allows applying specific queuing disciplines ([`qdisc`](https://tldp.org/HOWTO/Traffic-Control-HOWTO/classless-qdiscs.html)), such as [`netem`](https://man7.org/linux/man-pages/man8/tc-netem.8.html), to a target virtual interface in order to simulate network problems.
{{% /callout %}}

### Simulating Network Delays
To simulate network delays, you can use the `delayContainer` operation. This requires a `delay` argument specifying the amount of latency to apply to the container's outgoing traffic, for example `100ms` for 100 milliseconds of latency.
The delay must be formatted as a [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration).

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "delayContainer"
    args:
      containerId: "container-id|container-name"
      delay: "100ms"
      netInterface: "eth0" # optional argument, defaults to "eth0"
```

### Simulating Packet Loss
To simulate packet loss, you can use the `packetLossContainer` operation. This requires a `loss` argument specifying the percentage of packets to drop, for instance `10` or `10%` to drop 10% of the packets.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "packetLossContainer"
    args:
      containerId: "container-id|container-name"
      loss: "10%"
```

### Simulating Network Issues with Custom Qdiscs
It is possible to simulate more complex network conditions using the `customQdiscContainer` operation, which executes a custom command inside the container leveraging `tc` to enforce the desired network configuration.
Note that the command specified in the `qdiscCmd` parameter is executed in the target container using `tc qdisc add dev <netInterface> root <qdiscCmd>`. Therefore, you can apply any `tc`-compatible `qdisc` implementations, which can be found [here](https://man7.org/linux/man-pages/man8/tc.8.html#CLASSLESS_QDISCS).
Once a `qdisc` implementation is chosen, it is essential to consult its official documentation to find out which parameters and commands are supported.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "customQdiscContainer"
    args:
      containerId: "container-id|container-name"
      qdiscCmd: "tbf rate 10mbit burst 32kbit latency 400ms"
```

{{% callout type="warning" %}}
**Qdisc** is a scheduler that manages the queue of **outgoing** packets on a network interface and allows applying different traffic control policies. However, you might also notice impacts on **incoming** traffic because certain network protocols, such as TCP, require acknowledgments from the recipient to complete communication. Therefore, applying an outbound bandwidth limitation rule may indirectly affect inbound traffic as well.
{{% /callout %}}
