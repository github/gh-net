## Codespaces Network Bridge

🧪 *The extension is currently in the Preview stage, so some hiccups are expected. Please help us to improve [by submitting feedback](https://github.com/github/gh-net#troubleshooting)!*

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

This [GitHub CLI](https://cli.github.com/) extension allows you to bridge the network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In other words, it uses your local machine as a network `gateway` to get to those resources.

For instance, if you are using a `VPN` client to connect to private enterprise network to access a database or any other remote resources, this extension enables you to use those private resources from within a Codespace.

[About GitHub CLI](https://cli.github.com/).

## Prerequisites

1. This extension requires [GitHub CLI](https://cli.github.com/) version `v2.8.0` and up. Please make sure [to upgrade it](https://github.com/cli/cli#installation).

2. **If using GitHub CLI < [2.13.0](https://github.com/cli/cli/releases/tag/v2.13.0) only.** The extension relies on `gh codespace ssh` command to establish SSH tunnel to a Codespace. If you use [GitHub CLI >=2.13.0](https://github.com/cli/cli/releases/tag/v2.13.0) the `SSH` config is created automatically for all your Codespaces, otherwise follow [SSH setup](./docs/SSH_SETUP.md) instructions.

3. If your Codespace uses a non-default image, ensure that both the [GitHub CLI](https://cli.github.com/), `openssh-server`, and `sudo` are installed inside the codespace. Some distros need an `ssh` group too. Please see [linux dependencies doc](./docs/LINUX_DEPENDENCIES.md) for per-distro instructions.

## Installation

```shell
gh extension install github/gh-net
```

## Usage

To start network forwarding from a Codespace to a local machine, run:

```shell
gh net
```

> Note: on Windows, you need to use a command prompt launched with Administrator privileges.

Connection issues? Please see https://github.com/github/gh-net/issues/9 and [SSH setup doc](./docs/SSH_SETUP.md) for some of the known solutions.

The command will first open a Codespace selection dialog:

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161616184-4cd42419-6d97-440c-bf26-cb713baa7607.png">

Select a codespace and press enter. The extension will connect to selected codespace and start forwarding network traffic:

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

There are two panels in the connected view of the extension:

- Panel on the left (`NAT`) shows the network address translation table for currently opened connections. For stateful protocols(e.g. `TCP`) the records are cleaned up automatically after a connection is closed, so the records will come and go as connections are established and teardown. For stateless protocols (e.g. `UDP` or `ICMP`) or unsuccessful `TCP` connections, the records are cleaned up after a delay; hence those may show up in the list for some time.
- Panel on the right (`DNS`) shows the resolved `DNS` records, as `hostname`, `record`, and `time-to-live` (`TTL`) values.

Press `q` or `ctrl + c` to stop the extension.

### CLI Options

- `--gui`(`-g`): Enable/disable GUI mode. [`true` | `false`] [default: `true`]
- `--trace`(`-t`): Specify tracing verbosity. [`none` | `trace` | `debug` | `info` | `warn` | `error`] [default: `info`]
- `--trace-dest`: Specify tracing destination file. [`file name`] [default: `none`]
- `--dns`(`-d`): Enable/disable DNS resolution. [`true` | `false`] [default: `true`]
- `--codespace`(`-c`): Codespace name to connect to. [`codespace name`] [default: `none`]
- `--telemetry`: Enable/disable sending diagnostics telemetry (no `PII` data is sent). [`true` | `false`] [default: `true`]

Run `gh net -h` for details.

## Supported platforms

### Mac OSx

| OS                 | Local                             |
|--------------------|-----------------------------------|
| Big Sur (v11)      | <span title="supported">✅</span> |
| Monterey (v12)     | <span title="supported">✅</span> |

### Linux

| Distro                  | Local    | Inside Codespace                                           |
|-------------------------|----------|------------------------------------------------------------|
| Ubuntu                  | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Debian                  | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Fedora                  | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Red Hat                 | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Mint                    | <span title="supported">✅</span> | <span title="supported">✅</span> |
| OpenSUSE                | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Centos                  | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Kali                    | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Arch                    | <span title="supported">✅</span> | <span title="supported">✅</span> |
| Alpine                  | <span title="supported">✅</span> | <a href="https://github.com/github/gh-net/issues/12" title="supported">✅ *</a> |

### Windows

| Version                 | Local                               |
|-------------------------|-------------------------------------|
| Windows 10              | <span title="in progress">🏃</span> |
| Windows 11              | <span title="in progress">🏃</span> |

## Supported architectures

### Mac OSx

| Architecture                  | Status                            |
|-------------------------------|-----------------------------------|
| Intel(AMD64)                  | <span title="supported">✅</span> |
| Apple silicon (ARM64)         | <a href="https://github.com/github/gh-net/issues/22" title="supported">✅ *</a> |

### Linux

| Architecture            | Status                            |
|-------------------------|-----------------------------------|
| AMD64                   | <span title="supported">✅</span> |
| ARM64                   | <span title="supported">✅</span> |
| ARMv6                   | <span title="supported">✅</span> |
| ARMv7                   | <span title="supported">✅</span> |

### Windows

| Architecture            | Status                              |
|-------------------------|-------------------------------------|
| Windows 10 (AMD)        | <span title="supported">✅</span>   |
| Windows 11 (AMD)        | <span title="supported">✅</span>   |
| Windows 11 (ARM)        | <span title="in progress">🏃</span> |
| Windows 10 (i386)       | <span title="in progress">🏃</span> |

## Tested VPN Clients

| Name                    | Status                            |
|-------------------------|-----------------------------------|
| Viscocity               | <span title="supported">✅</span> |
| GlobalProtect           | <span title="supported">✅</span> |
| NordVPN                 | <span title="supported">✅</span> |
| Tailscale               | <span title="supported">✅</span> |

<br />

Legend: ✅ - supported 🏃 - in progress `?` - unknown / not tested

<br />

For list of supported network protocols refer to [this doc](./docs/SUPPORTED_NETWORK_PROTOCOLS.md).

## Troubleshooting

- Something is missing? Please create a [✨ feature request](https://github.com/github/gh-net/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=).
- Something is incorrect? Please create a [🐛 bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).
- For known issues, refer to [👉 this list](https://github.com/github/gh-net/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue).

## Useful links

- [How it works](./docs/HOW_IT_WORKS.md)
- [About GitHub CLI](https://cli.github.com/)
- [About GitHub Codespaces](https://github.com/features/codespaces)
- [🔒 Source code](https://github.com/github/codespaces-vpn-gateway)
