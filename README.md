## Codespaces Network Bridge GitHub CLI extension

This [GitHub CLI](https://cli.github.com/) extension allows to bridge network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In another words, it uses your local machine a network `gateway` to get to those resources.

For instance, if you are using `VPN` to connect to your enterprise network to access a database or any other remote resources on the private network, this extension allows you to get to those resources from whithin a Codespace also, so you can develop fully inside a Codespace!

[About GitHub CLI](https://cli.github.com/)

## Installation

```shell
gh extension install legomushroom/gh-net
```

*This extension depends on the latest features of GitHub CLI, please make sure [to upgrade it](https://github.com/cli/cli#installation).*

<details>
    <summary>How to check if my version works?</summary>
    Run `gh codespace select` command, if it opens the codespace selection dialog, you are good to go!
    <img width="749" alt="image" src="https://user-images.githubusercontent.com/1478800/161620032-c999de5a-7445-4662-bedd-95be830556e9.png">
</details>

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

- Panel on the left (`NAT`) shows the network address translation table for currently opened connections. For successful stateful protocols(e.g. `TCP`) the records are cleaned up automatically after connection is closed so teh records will come and go as connection is established and closed. For stateless protocols (e.g. `UDP` or `ICMP`) or unsuccessful `TCP` connections the records are cleaned up after some time, so those will show up in the list for some time.
- Panel on the right (`DNS`) shows the resolved `DNS` records, as `hostname`, `record` and `time-to-live` (or `TTL`) values.

Press `q` or `ctrl + c` to stop the extension.

## How it works

General diagram is shown below:

<img src="./diagrams/general.png" alt="general schema" />

We bind to the `default gateway` network interface inside the codespace and forward all non-routed traffic to the `SSH tunnel` that connects a Codespace with your local machine. We forward only `L3` (`IP`) traffic and there are few criterias must hold for traffic to be forwarded:

- it must appear on the `default gateway`
- it must not be addressed tosomething that is on default gateway subnet

This ensures that we fallback to forwaring packets only if they were not handled by any other network interface inside a Codespace.

Once a packet reaches the local machine, we see if we can forward it to a meaningful distanation, for that we resolve network interface that can handle the packet destination. Such network interface must not be a default gateway interface given that the packet destination is not to the gateway subnet, otherwise the packet is addressed to internet which can be handled from whithin the codespace directly.

If such network interface is found, we create a local `network socket` and a `NAT` record for the connection. The NAT reord is used to map remote packet source address to the local network socket address, so it appears to the remote resource as if traffic is comming from the local machine. When a reply packet is received, we perform reverse address translation and send the packet back to the codespace (so it appears if reply came directly from the Codespace default gateway interface).

For `DNS` packets, we register an address that is on `default gateway` subnet which allows to catch all unresolved `DNS` queries. Once `DNS` packet received, it is passed over to the `local machine` where request to local `DNS` resolver is made and reply sent back to the Codespace.

## Troubleshooting

Troubleshooting notes.

## Supported platforms

| Target platforms        | Local | Inside Codespace |
|-------------------------|-------|--------------|
| Mac OSx (Intel)         | ✅     | 🙅          |
| Mac OSx (Apple)         | 🏃     | 🙅          |
| Linux (Ubuntu)          | ✅     | ✅            |
| Linux (Debian)          | ✅     | ✅            |
| Linux (Fedora)          | ✅     | ✅            |
| Linux (Red Hat)         | ✅     | ✅            |
| Linux (Mint)            | ✅     | ✅            |
| Linux (OpenSUSE)        | ✅     | ✅            |
| Linux (Centos)          | ✅     | ✅            |
| Linux (Kali)            | ✅     | ✅            |
| Linux (Raspberry Pi OS) | ✅     | ✅            |
| Windows 10              | 🏃     | 🙅          |

✅ - currently supported 🏃 - support in progress 🙅 - not applicable

### DNS

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

## Other GitHub CLI extensions for Codespaces

- [Codespace Compose](https://github.com/github/gh-codespace-compose)
- [About GitHub CLI](https://cli.github.com/)
- [GitHub CLI Docs](https://cli.github.com/manual/gh)

## License

License text.
