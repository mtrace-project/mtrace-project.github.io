---
title: Docker
weight: 2
cascade:
  type: docs
---

The *docker* setup command allows you to perform certain types of operations inside Docker containers, such as stopping or slowing down a container before the execution of a test.

This type of command supports the operations detailed below.

## Container state operations
These operations allow you to modify the state of a Docker container, for example, to simulate a service interruption.

| Operation          | Description                                                                                 |
| ------------------ | ------------------------------------------------------------------------------------------- |
| `killContainer`    | Terminates the execution of the container without removing it, by sending a **SIGKILL** signal |
| `stopContainer`    | Stops the execution of the container without removing it, by sending a **SIGTERM** signal      |
| `startContainer`   | Starts the execution of the container                                                       |
| `pauseContainer`   | Pauses the execution of the container                                                       |
| `unpauseContainer` | Resumes the execution of the container                                                      |

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "stopContainer"
    args:
      containerId: "container-id|container-name"
```
All the above operations require a `containerId` argument, which can be the identifier or the name of the container on which the operation should be executed.

## Command execution inside a container
This operation allows you to execute a command inside a Docker container, for example, to apply specific configurations before the execution of a test.   
The operation is defined with the name `execContainer` and requires two arguments: `containerId` and `cmd`, which is the command to be executed inside the container.   
Furthermore, it requires a cleanup command of the same type, which allows you to execute a command inside the same container after the test execution, for instance, to restore the configurations modified by the setup command.

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

## Docker Compose container startup
This operation allows you to start the containers defined in a given **Docker Compose** configuration file and wait for all of them to be running or in a *healthy* state before proceeding with the test execution or subsequent setup commands.   
The operation is defined with the name `composeUp` and requires a `composePath` argument, which represents the path to the **Docker Compose** configuration file, and an optional `projectName` argument that allows you to specify the name of the Docker Compose project; otherwise, the name of the folder containing the configuration file will be used.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "composeUp"
    args:
      composePath: "docker-compose.yaml"
      projectName: "my-project" # optional argument, default: name of the folder containing the configuration file
```

## Network issue simulation inside a container
These operations allow you to simulate network problems inside a Docker container, for instance by injecting latency or limiting the available bandwidth. These types of operations require a `containerId` and specific arguments. 

Additionally, you can specify the virtual interface on which to apply the operation using the `netInterface` argument; otherwise, the default `eth0` interface will be used.    
In most cases, a Docker container without specific network configurations uses `eth0` as the default interface; however, you can verify the interface name by running the `ip addr` command inside the container.

{{% callout type="info" %}}
**Mtrace** bases this feature on the use of [`tc`](https://man7.org/linux/man-pages/man8/tc.8.html), a Linux tool for configuring the network subsystem. It allows you to apply specific [`qdisc`](https://tldp.org/HOWTO/Traffic-Control-HOWTO/classless-qdiscs.html) implementations, such as [`netem`](https://man7.org/linux/man-pages/man8/tc-netem.8.html), to the specified virtual interface in order to simulate network issues.
{{% /callout %}}

### Network delay simulation
To simulate network delays, you can use the `delayContainer` operation. This operation requires a `delay` argument that specifies the amount of latency to apply to the outbound traffic of the container, for instance `100ms` for 100 milliseconds of latency.   
The delay must be specified in the [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration) format.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "delayContainer"
    args:
      containerId: "container-id|container-name"
      delay: "100ms"
      netInterface: "eth0" # optional argument, default "eth0"
```

### Packet loss simulation
To simulate packet loss, you can use the `packetLossContainer` operation. This operation requires a `loss` argument that specifies the percentage of packets to drop, for instance `10` or `10%` to drop 10% of the packets.

Example:
```yaml
setupCommands:
  - type: "docker"
    cmd: "packetLossContainer"
    args:
      containerId: "container-id|container-name"
      loss: "10%"
```

### Custom qdisc network issue simulation
You can simulate more complex network issues by using the `customQdiscContainer` operation to execute a custom command inside the container that leverages `tc` to apply the desired network configuration.   
It is important to consider that the command specified in the `qdiscCmd` parameter is executed in the target container along with `tc qdisc add dev <netInterface> root <qdiscCmd>`; therefore, you can apply any `qdisc` implementations compatible with `tc`, which can be found [here](https://man7.org/linux/man-pages/man8/tc.8.html#CLASSLESS_QDISCS).   
Once you have chosen the `qdisc` implementation to use, it is crucial to consult the official documentation of the individual implementation to discover the supported parameters and commands.

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
**Qdisc** is a scheduler that manages the queue of packets **exiting** a network interface and allows you to apply different traffic management policies. However, you might also notice the applied rules affecting **incoming** traffic because some network protocols, such as TCP, require acknowledgment from the recipient to complete the communication; therefore, if you apply an outbound bandwidth limitation rule, this could indirectly affect the incoming traffic as well.
{{% /callout %}}
