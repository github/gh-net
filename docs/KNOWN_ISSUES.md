## Known issues

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

