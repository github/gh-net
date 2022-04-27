## Codespaces Network Bridge

🧪 *The extension is currently in Preview stage, so some hiccups are expected. Please help us to improve [by submitting feedback](https://github.com/github/gh-net#troubleshooting)!*

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

This [GitHub CLI](https://cli.github.com/) extension allows to bridge network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In another words, it uses your local machine as a network `gateway` to get to those resources.

For instance, if you are using a `VPN` to connect to your enterprise network to access a database or any other remote resources on the private network, this extension allows you to get to those resources from within a Codespace, so that you can develop fully inside a Codespace!

[About GitHub CLI](https://cli.github.com/).

## Prerequisites


1. This extension depends on the latest features of GitHub CLI(>= v2.8.0), please make sure [to upgrade it](https://github.com/cli/cli#installation). If run the `gh codespace select` command opens the codespace selection dialog, you are good to go.

2. The extension relies on `gh codespace ssh` command to establish SSH tunnel to a Codespace, hence you need to setup ssh keys if you didn't before. If you can do `sudo gh codespace ssh` (`sudo` is important since kernel might use different identity under non-root user) and connect to a Codespace successfully, - you are good to go. Refer to [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for more info. List of known issues and workarounds for them can be found [here](https://github.com/github/gh-net/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue).

3. If your codespace uses a non-default image, ensure that both the [GitHub CLI](https://cli.github.com/) and an SSH server are installed inside the codespace. 

One way to install the prereqs is by adding the following to the `features` property of your `devcontainer.json` file.
```jsonc
// devcontainer.json
{
    "features": {
        "github-cli": "latest",
        "sshd": "latest"
    }
}
```

Rebuild the codespace to apply the newly added dev container features.



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

> ⚠️ Note: If you set a `passphrase` for your `SSH` keys, you will need to pass `--gui false` CLI option, otherwise you won't be able to connect. This will be fixed in future releases.

This will provide codespace selection dialog:

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161616184-4cd42419-6d97-440c-bf26-cb713baa7607.png">

Select a codespace and press enter. The extension will connect to selected codespace and start forwarding network traffic:

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

There are two pannels in the connected view of the extension:

- Panel on the left (`NAT`) shows the network address translation table for currently opened connections. For stateful protocols(e.g. `TCP`) the records are cleaned up automatically after connection is closed so the records will come and go as connection is established and closed. For stateless protocols (e.g. `UDP` or `ICMP`) or unsuccessful `TCP` connections the records are cleaned up after some time, so those will show up in the list for some time.
- Panel on the right (`DNS`) shows the resolved `DNS` records, as `hostname`, `record` and `time-to-live` (`TTL`) values.

Press `q` or `ctrl + c` to stop the extension.

### CLI Options

- `--gui`: Enanble/disable GUI mode. [`true` | `false`] [default: `true`]
- `--dns`: Enanble/disable DNS resolution. [`true` | `false`] [default: `true`]
- `--trace`: Specify tracing verbosity. [`none` | `trace` | `debug` | `info` | `warn` | `error`] [default: `info`]

Run `gh net start -h` for details.


## Supported platforms

### Mac OSx

| Architecture            | Local |
|-------------------------|-------|
| Intel                   | ✅     |
| Apple silicon           | ✅     |

### Linux

| Distro                  | Local | Inside Codespace |
|-------------------------|-------|------------------|
| Ubuntu                  | ✅     | ✅               |
| Debian                  | ✅     | ✅               |
| Fedora                  | ?      | ✅               |
| Red Hat                 | ?      | ✅               |
| Mint                    | ?      | ✅               |
| OpenSUSE                | ?      | ✅               |
| Centos                  | ?      | ✅               |
| Kali                    | ?      | ✅               |
| Arch                    | ?      | ✅               |
| Alpine                  | ?      | ✅               |

### Windows

| Version                 | Local |
|-------------------------|-------|
| Windows 10              | 🏃     |
| Windows 11              | 🏃     |

## VPN Clients

| Name                    | Status |
|-------------------------|--------|
| Viscocity               | ✅     |
| GlobalProtect           | ✅     |
| NordVPN                 | ✅     |
| Tailscale               | ✅     |

<br />

Legend: ✅ - currently supported 🏃 - in progress `?` - unknown / not tested

<br />

For list of supported network protocols refer to [this doc](./docs/SUPPORTED_NETWORK_PROTOCOLS.md).

## Troubleshooting

- Something is missing? Please create a [✨ feature request](https://github.com/github/gh-net/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=).
- Something is incorrect? Please create a [🐛 bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).
- For list of known issues refer to [👉 this doc](./docs/KNOWN_ISSUES.md) or [👉 this list](https://github.com/github/gh-net/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue).

## Useful links

- [How it works](./docs/HOW_IT_WORKS.md)
- [About GitHub CLI](https://cli.github.com/)
- [About GitHub Codespaces](https://github.com/features/codespaces)
- [🔒 Source code](https://github.com/github/codespaces-vpn-gateway)
