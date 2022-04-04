## Codespaces Network Bridge GitHub CLI extension

This [GitHub CLI](https://cli.github.com/) extension allows to bridge network between a Codespace and your local machine, so the `Codespace` can reach out to any remote resource that is reachable from your machine. In another words, it uses your local machine a network `gateway` to get to those resources.

For instance, if you are using `VPN` to connect to your enterprise network to access a database or any other remote resources on the private network, this extension allows you to get to those resources from whithin a Codespace also, so you can develop fully inside a Codespace!

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

How it works notes.

## Troubleshooting

Troubleshooting notes.

## Supported platforms

- MacOSx AMD64
- Linux AMD64

[About GitHub CLI](https://cli.github.com/)
