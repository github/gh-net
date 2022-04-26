## Known issues

- 🚩 [#6 [mac osx] Do you want the application "gh net" to accept incoming network connections? come and go out quickly](https://github.com/github/gh-net/issues/6)

### 🚩 SSH stack trace


### 🚩 I'm getting an `Permissions denied` error an a stack trace immediatelly after starting the extension

- Most likely you forgot to use `sudo` to run the extension. Sudo is required to bind to network sockets on the local machine. If you still seeing the error after running the same command with `sudo`, please create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).

### 🚩 My local machine network configuration has changed but extension does not pick up the changes

- Please restart the extension by pressing `q` and connecting to the Codespace again. The extension currently does not watch for changes in network configuration and hence does not detect new network interfaces or changes in interfaces config. This will be fixed in the future.

### 🚩 [Alpine Linux] DNS resolution inside an `Alpine Linux`-based Codespace sporadically fails

Apline Linux kernel queries a random known nameserver as oppose to doing queries in sequence as other Linux distros would do. This means that sometimes DNS requests go past extension-defined nameserver so DNS resolution will fail.

Known workarounds:

- add explicit `ip-address hostname` mapping to `/etc/hosts` file inside a Codespace
- modify the `/etc/resolv.conf` file to keep only the VPN Gateway-defined nameserver (one marked with `# Codespaces VPN Gateway` comment)

### 🚩 Extension suddenly stops working after some time and I see some stack traces in the console

Most likely `SSH` connection was dropped or there was an intermittent network issue on your machine. The extension does not currently reconnects to the Codespace automatically. This will be fixed in the future. If this happens too often, please create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=).

### 🚩 I'm trying to send `Ethernet Datagrams`(L2 network layer) directly and expect those to be forwarded but they are not

The extension currently forwards `IP`(L3 network layer) traffic and above. If the datagrams contain `IP` packets that are addressed to a remote resource addressible from your local machine it should work. If it does not, please create a [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=). If you want to send `Ethernet Datagrams` directly, please create a [Feature request](https://github.com/github/gh-net/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=), we would love to know about your use case!

### 🚩 I'm using some transport protocol that does not work

Currently `TCP`/`UDP` and `ICMP` are supported. Other protocols should work but were not tested extensivelly. Please create [Bug report](https://github.com/github/gh-net/issues/new?assignees=&labels=bug&template=bug_report.md&title=) so we can address the issue.

