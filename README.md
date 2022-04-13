## Codespaces Network Bridge

🧪 *The extension is currently in Preview stage, so some hiccups are expected. Please help us to improve [by submitting feedback](https://github.com/github/gh-net#troubleshooting)!*

<img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161617508-b65de564-60f3-46c8-8394-5b28c8ac477b.png">

This [GitHub CLI](https://cli.github.com/) extension allows to bridge network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In another words, it uses your local machine as a network `gateway` to get to those resources.

For instance, if you are using a `VPN` to connect to your enterprise network to access a database or any other remote resources on the private network, this extension allows you to get to those resources from within a Codespace, so that you can develop fully inside a Codespace!

[About GitHub CLI](https://cli.github.com/).

## Installation

*This extension depends on the latest features of GitHub CLI, please make sure [to upgrade it](https://github.com/cli/cli#installation).*

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

You need to use `sudo` during extension installation on Linux due to https://github.com/cli/cli/issues/5456. Hopefully it won't be the case in the future.

⚠️ If your Codespaces use non-default image, make sure that [GitHub CLI](https://cli.github.com/) is installed inside the Codespaces. The installation step can happen either in `Dockerfile` or in one of the `.devcontainer.json` lifecycle hooks e.g. [postCreateCommand](https://code.visualstudio.com/docs/remote/devcontainerjson-reference). See https://github.com/cli/cli#installation for GitHub CLI installation instructions.

<br />

Upgrading the extension (you need `sudo` on Linux):

```shell
gh extension upgrade github/gh-net
```

## Usage

To start network forwarding from whithin a Codespace to your local machine, run:

```shell
sudo gh net start
```

> Note: `sudo` privileges are required to bind to network sockets on your machine. In theory user with network administration rights can run this command without sudo, but this scenario is not tested well enough yet, so using `sudo` is the hard requirement at the moment.

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

## How it works

General diagram is shown below:

<img src="./diagrams/general.png" alt="general schema" />

We bind to the `default gateway` network interface inside the codespace and forward all non-routed traffic to the `SSH tunnel` that connects a Codespace with your local machine. We forward only `L3` (`IP`) traffic and there are few criterias must hold for traffic to be forwarded:

- it must appear on the `default gateway`
- it must not be addressed tosomething that is on default gateway subnet

This ensures that we fallback to forwaring packets only if they were not handled by any other network interface inside a Codespace.

Once a packet reaches the local machine, we see if we can forward it to a meaningful destination, for that we resolve network interface that can handle the packet destination. Such network interface must not be a default gateway interface given that the packet destination is not to the gateway subnet, otherwise the packet is addressed to the internet which can be handled from whithin the codespace directly.

If such network interface is found, we create a local `network socket` and a `NAT` record for the connection. The NAT record is used to map the remote packet source address to the local network socket address, so it appears to the remote resource as if traffic is coming from the local machine. When a reply packet is received, we perform reverse address translation and send the packet back to the codespace (so it appears as if the reply came directly from the codespace default gateway interface).

For `DNS` packets, we register an address that is on the `default gateway` subnet which allows to catch all unresolved `DNS` queries. Once `DNS` packet is received, it is passed over to the `local machine` where the request to the local `DNS` resolver is made and a reply is sent back to the codespace.

The extension is written in `Rust` and provides high preformance, low memory footprint and memory safety, hence must cause low latency.

## Supported platforms

| Target platforms        | Local | Inside Codespace |
|-------------------------|-------|--------------|
| Mac OSx (Intel)         | ✅     | 🙅          |
| Mac OSx (Apple)         | 🏃     | 🙅          |
| Linux (Ubuntu)          | ✅     | ✅          |
| Linux (Debian)          | ✅     | ✅          |
| Linux (Fedora)          | ?      | ?          |
| Linux (Red Hat)         | ?      | ?          |
| Linux (Mint)            | ?      | ?          |
| Linux (OpenSUSE)        | ?      | ?          |
| Linux (Centos)          | ?      | ?          |
| Linux (Kali)            | ?      | ?          |
| Linux (Raspberry Pi OS) | ?      | ?          |
| Windows 10              | 🏃     | 🙅          |

✅ - currently supported 🏃 - support in progress 🙅 - not applicable

### DNS Record Type Support

| DNS Record Type | Status |
|-----------------|--------|
| A               | ✅      |
| AAAA            | ✅      |
| CNAME           | ✅      |
| NS              | ✅      |
| TXT             | ✅      |
| SOA             | ✅      |
| PTR             | ✅      |
| NULL            | ✅      |
| MX              | ✅      |
| ANY             | ✅      |

### Transport layer protocol support

Currently only `TCP`, `UDP` and `ICMP` protocols were tested extensively:

| Transport protocol | Status |
|--------------------|--------|
| TCP                | ✅     |
| UDP                | ✅     |
| ICMP               | ✅     |
| SCTP               | ?      |
| DCCP               | ?      |
| RSVP               | ?      |
| QUIC               | ?      |

### Network layer protocol support

Currently only `IPv4` is supported and was tested extensively:

| Network protocol   | Status |
|--------------------|--------|
| IPv4               | ✅     |
| IPv6               | ?      |
| IGMP               | ?      |
| NDP                | ?      |
| ECN                | ?      |
| IPSec              | ?      |

## Troubleshooting

- To create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).
- To create a [Feature request](https://github.com/github/gh-net/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=).

Please search for existing issues before creating a new one.

### Known issues

> My local machine network configuration has changed but extension does not pick up the changes.

- Please restart the extension by pressing `q` and connecting to the Codespace again. The extension currently does not watch for changes in network configuration and hence does not detect new network interfaces or changes in interfaces config. This will be fixed in the future.

> I'm getting an error an a stack trace immediatelly after starting the extension.

- Most likely you forgot to use `sudo` to run the extension. If `sudo` was used, please create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).

> Extension suddenly stops working after some time and I see some stack traces in the console.

Most likely `SSH` connection was dropped or there was an intermittent network issue on your machine. The extension does not currently reconnects to the Codespace automatically. This will be fixed in the future. If this happens too often, please create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).

> I'm trying to send `Ethernet Datagrams`(L2 network layer) directly and expect those to be forwarded but they are not.

The extension currently forwards `IP`(L3 network layer) traffic and above. If the datagrams contain `IP` packets that are addressed to a remote resource addressible from your local machine it should work. If it does not, please create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=). If you want to send `Ethernet Datagrams` directly, please create a [Feature request](https://github.com/github/gh-net/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=), we would love to know about your use case!

> I'm using some transport protocol that does not work.

Currently `TCP`/`UDP` and `ICMP` are supported. Other protocols should work but were not tested extensivelly. Please create [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=) so we can address the issue.

## Useful links

- [About GitHub CLI](https://cli.github.com/)
- [GitHub CLI Docs](https://cli.github.com/manual/gh)
- [About GitHub Codespaces](https://github.com/features/codespaces)
- [🔒 Source code](https://github.com/github/codespaces-vpn-gateway)
- [🔒 Codespace Compose GitHub CLI extension](https://github.com/github/gh-codespace-compose)



