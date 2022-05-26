## Codespaces Network Bridge

🧪 *The extension is currently in Preview stage, so some hiccups are expected. Please help us to improve [by submitting feedback](https://github.com/github/gh-net#troubleshooting)!*

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

This [GitHub CLI](https://cli.github.com/) extension allows to bridge network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In another words, it uses your local machine as a network `gateway` to get to those resources.

For instance, if you are using a `VPN` client to connect to private enterprise network to access a database or any other remote resources, this extension enables you to use those private resources from within a Codespace.

[About GitHub CLI](https://cli.github.com/).

## Prerequisites


1. This extension depends on the latest features of GitHub CLI(>= v2.8.0), please make sure [to upgrade it](https://github.com/cli/cli#installation). If run the `gh codespace select` command opens the codespace selection dialog, you are good to go.

2. The extension relies on `gh codespace ssh` command to establish SSH tunnel to a Codespace, hence you need to setup ssh keys if you didn't before. If you can do `sudo gh codespace ssh` (`sudo` is important since kernel might use different identity under non-root user) and connect to a Codespace successfully, - you are good to go. Refer to [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for more info. List of known issues and workarounds for them can be found [here](https://github.com/github/gh-net/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue).

3. If your codespace uses a non-default image, ensure that both the [GitHub CLI](https://cli.github.com/), `openssh-server` and `sudo` are installed inside the codespace. Some distros need an `ssh` group too. Please see [linux dependencies doc](./docs/LINUX_DEPENDENCIES.md) for per-distro instructions.

## Installation

Mac OSx:

```shell
gh extension install github/gh-net
```

Linux:

```shell
sudo gh extension install github/gh-net
```

The `sudo` is required during extension installation on Linux due to https://github.com/cli/cli/issues/5456. Hopefully it won't be the case in the future.

## Usage

To start network forwarding from a Codespace to a local machine, run:

```shell
sudo gh net start
```

Connection issues? Please see https://github.com/github/gh-net/issues/9 for some common solutions.

This will provide codespace selection dialog:

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161616184-4cd42419-6d97-440c-bf26-cb713baa7607.png">

Select a codespace and press enter. The extension will connect to selected codespace and start forwarding network traffic:

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

There are two panels in the connected view of the extension:

- Panel on the left (`NAT`) shows the network address translation table for currently opened connections. For stateful protocols(e.g. `TCP`) the records are cleaned up automatically after connection is closed so the records will come and go as connection is established and closed. For stateless protocols (e.g. `UDP` or `ICMP`) or unsuccessful `TCP` connections the records are cleaned up after some time, so those will show up in the list for some time.
- Panel on the right (`DNS`) shows the resolved `DNS` records, as `hostname`, `record` and `time-to-live` (`TTL`) values.

Press `q` or `ctrl + c` to stop the extension.


### CLI Options

- `--gui`: Enable/disable GUI mode. [`true` | `false`] [default: `true`]
- `--dns`: Enable/disable DNS resolution. [`true` | `false`] [default: `true`]
- `--trace`: Specify tracing verbosity. [`none` | `trace` | `debug` | `info` | `warn` | `error`] [default: `info`]
- `--telemetry`: Enable/disable sending diagnostics telemetry (no `PII` data is sent). [`true` | `false`] [default: `true`]

Run `gh net start -h` for details.


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
| AMD64                   | <span title="in progress">🏃</span> |
| i386                    | <span title="in progress">🏃</span> |

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
- For list of known issues refer to [👉 this list](https://github.com/github/gh-net/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue).

## Useful links

- [How it works](./docs/HOW_IT_WORKS.md)
- [About GitHub CLI](https://cli.github.com/)
- [About GitHub Codespaces](https://github.com/features/codespaces)
- [🔒 Source code](https://github.com/github/codespaces-vpn-gateway)
