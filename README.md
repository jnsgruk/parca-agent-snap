# Parca Agent Snap

This repository contains the source of an unofficial snap for the [Parca Agent](https://parca.dev).

Parca Agent is an always-on sampling profiler that uses eBPF to capture raw profiling data with
very low overhead. It observes user-space and kernel-space stacktraces 100 times per second and
builds pprof formatted profiles from the extracted data. Read more details in the design
documentation.

The collected data can be viewed locally via HTTP endpoints and then be configured to be sent to a
Parca server to be queried and analyzed over time.

## Parca Agent App

The snap provides a base `parca-agent` app, which can be executed as per the upstream
documentation.

You can start Parca Agent manually like so:

```bash
# Install from the 'edge' channel
$ sudo snap install parca-agent --channel edge

# For now, until auto-connect requests are granted
sudo snap connect parca-agent:system-observe
sudo snap connect parca-agent:system-trace

# Start the agent with simple defaults for testing
parca-agent --node="foobar" --store-address="localhost:7070" --insecure
```

## Parca Agent Service

Additionally, the snap provides a service for Parca Agent with a limited set of configuration
options. You can start the service like so:

```bash
$ snap start parca-agent
```

There are a small number of config options:

| Name            | Valid Options                    | Default          | Description                                                             |
| :-------------- | :------------------------------- | :--------------- | :---------------------------------------------------------------------- |
| `node`          | Any string                       | `$(hostname)`    | Name node the process is running on.                                    |
| `log-level`     | `error`, `warn`, `info`, `debug` | `info`           | Log level for Parca                                                     |
| `http-address`  | Any string                       | `:7071`          | Address for HTTP server to bind to                                      |
| `store-address` | Any string                       | `localhost:7071` | gRPC address to send profiles and symbols to.                           |
| `insecure`      | `true`, `false`                  | `false`          | Send gRPC requests via plaintext instead of TLS.                        |
| `kubernetes`    | `true`, `false`                  | `false`          | Discover containers running on this node and profile them automatically |

Config options can be set with `sudo snap set parca-agent <option>=<value>`
