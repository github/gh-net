## Codespaces Network Bridge

🧪 *The extension is currently in Preview stage, so some hiccups are expected. Please help us to improve [by submitting feedback](https://github.com/github/gh-net#troubleshooting)!*

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

This [GitHub CLI](https://cli.github.com/) extension allows to bridge network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In another words, it uses your local machine as a network `gateway` to get to those resources.

For instance, if you are using a `VPN` to connect to your enterprise network to access a database or any other remote resources on the private network, this extension allows you to get to those resources from within a Codespace, so that you can develop fully inside a Codespace!

[About GitHub CLI](https://cli.github.com/).

## Installation

*This extension depends on the latest features of GitHub CLI(>= v2.8.0), please make sure [to upgrade it](https://github.com/cli/cli#installation).*

<details>
    <summary>How to check if my version works?</summary>
    Run `gh codespace select` command, if it opens the codespace selection dialog, you are good to go!
    <img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161620032-c999de5a-7445-4662-bedd-95be830556e9.png">
</details>

Mac OSx:

```shell
gh extension install github/gh-net
```

Linux:

```shell
sudo gh extension install github/gh-net
```

The `sudo` is required during extension installation on Linux due to https://github.com/cli/cli/issues/5456. Hopefully it won't be the case in the future.

⚠️ If your Codespaces use non-default image, make sure that [GitHub CLI](https://cli.github.com/) and SSH server are installed inside the Codespaces. It might be as simple as adding `"github-cli": "latest"` and `"sshd": "latest"` to the `"feartures"` section of the `devcontainer.json` file!

## Usage

To start network forwarding from a Codespace to a local machine, run:

```shell
sudo gh net start
```

> Note: `sudo` privileges are required to bind to network sockets on your machine.

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
- `--trace-level`: Specify tracing verbosity. [`none` | `trace` | `debug` | `info` | `warn` | `error`] [default: `info`]

Run `gh net start -h` for details.


## Supported platforms

### Mac OSx

| Architecture            | Local | Inside a Codespace |
|-------------------------|-------|--------------------|
| Intel                   | ✅     | 🙅                 |
| Apple                   | 🏃     | 🙅                 |

### Linux

| Distro                  | Local | Inside Codespace |
|-------------------------|-------|------------------|
| Ubuntu                  | ✅     | ✅               |
| Debian                  | ✅     | ✅               |
| Fedora                  | ?      | ?               |
| Red Hat                 | ?      | ?               |
| Mint                    | ?      | ?               |
| OpenSUSE                | ?      | ?               |
| Centos                  | ?      | ?               |
| Kali                    | ?      | ?               |
| Raspberry Pi OS         | ?      | ?               |
| Alpine (bullseye)       | ?      | ✅               |

### Windows

| Version                 | Local | Inside a Codespace |
|-------------------------|-------|--------------------|
| Windows 10              | 🏃     | 🙅                 |
| Windows 11              | 🏃     | 🙅                 |

<br />

Legend: ✅ - currently supported 🏃 - in progress 🙅 - not applicable `?` - unknown / not tested

<br />

For list of supported network protocols refer to [this doc](./docs/SUPPORTED_NETWORK_PROTOCOLS.md).

## Troubleshooting

- Something is missing? Please create a [✨ feature request](https://github.com/github/gh-net/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=).
- Something is incorrect? Please create a [🐛 bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).
- For list of known issues refer to [👉 this doc](./docs/KNOWN_ISSUES.md).

## Useful links

- [How it works](./docs/HOW_IT_WORKS.md)
- [About GitHub CLI](https://cli.github.com/)
- [About GitHub Codespaces](https://github.com/features/codespaces)
- [🔒 Source code](https://github.com/github/codespaces-vpn-gateway)
