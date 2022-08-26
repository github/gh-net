## 0.12.2

- adds `Windows 10` and `Windows 11` support for AMD-based machines.
- adds automatic `sudo` privileges escalation on `Unix`. No need to use `sudo` to install or launch the extension anymore.
- extension now fallbacks to `start` command if no command was specified, effectively making it the default command.
- makes `DNS` queries resolution process asynchronous so that `DNS` queries are not blocking other traffic.
- improve `network traffic` GUI panel rendering accuracy and performance.

## 0.8.3

- add `--codespace`(`-c`) CLI argument to specify a `Codespace` name to connect to. More info: https://github.com/github/gh-net/issues/30
- add `--trace-dest` CLI argument to specify trace file path. More info: https://github.com/github/gh-net/issues/35
- fix IP routing issue for PTP network interfaces that are configured to be a default gateway on local machine. More info: https://github.com/github/gh-net/issues/33
- improved start command panic handling.
- bug fixes for app diagnostics logic.

## 0.6.4

- increase number of tasks in worker thread pool to `12`. This fixes app freezes issue for low-end machines with few number of CPU cores and allowes for `11` active network interfaces on the host(local) machine. More info: https://github.com/github/gh-net/issues/7
- fix bad TCP socket state error for TCP NATs on Linux host(local) machines. More info: https://github.com/github/gh-net/issues/8
- improve error handling and visibility
- add `--telemetry` CLI option to enable/disable telemetry requests for app troubleshooting purposes. It has values of `true`/`false`, default is `true`.
- fix infinite loop for `DNS` resolution of `AAAA` records if host does not support `IPv4`-mapped `IPv6` `DNS` records. More info: https://github.com/github/gh-net/issues/18
- fix issue that caused `TCP`/`UDP` `NAT` records to be disposed prematurely. More info: https://github.com/github/gh-net/issues/17
- release extension for `ARM64`, `ARMv6` and `ARMv6` architectures for `Linux` machines.

## 0.4.0

- add `SRV` DNS records support which is commonly used by Linux package managers (e.g. `apt` or `apt-get`).
- DNS resolution now also relies on the `getaddrinfo` system call to support some of the VPN client setups(e.g. `Viscosity`, `Tailscale`). More info: https://github.com/github/gh-net/issues/4
- improve the speed of `DNS` resolution for `A`/`AAAA` DNS record types. Now resolution takes single-digit `milliseconds` as opposed to `seconds` as before.
- `GUI` changes:
  - entire `GUI` now expands to 100% of width by default to accommodate longer `DNS` hostnames.
  - `DNS` panel now includes the negative responses like `NXDOMAIN`, `NODATA`, or `SERVFAIL`.
  - improved `DNS` panel layout . Text items now fill out the entire panel width and have guiding lines for readability.
  - `NAT` panel now includes connection types, like `TCP`, `UDP`, or others.
  - improved `NAT` panel layout. Text items now fill out the entire panel width and have guiding lines for readability.
  - each connection in the `NAT` panel now includes `used ago` metric in seconds for `incoming` and `outgoing` network traffic.
- `start` command changes:
  - add `--dns` option which allows to enable/disable DNS resolution. It has values of `true`/`false`, default is `true`.
  - rename `--trace-level` option to `--trace` to be more consistent with other options. The `--trace-level` option is still supported as an alias for the new name but removed from docs and will be entirely removed in upcoming releases.
  - `--trace` option now gets propagated to the `gh net` extension inside a `Codespace` for tracing level parity.
  - add hidden `--repo` option, which controls what repo is used when the `gh net` extension is auto-installed inside a `Codespace` during the initial connection. This command is meant to be used for testing/debugging purposes; hence is `hidden`.
  - improve logic to auto-update `gh net` extension on the remote side (inside a Codespace). This logic ensures that the remote extension is always up to date.
  - add GitHub CLI update notice if CLI is `< v2.8.0` instead of failing with a generic error.
  - provide clear error messages for other expected error cases.

## 0.3.2

- fix automatic `gh net` installation for root vs non-root users on Linux. See https://github.com/cli/cli/issues/5456
- ask users to install GitHub CLI if it is not installed inside a remote machine
- pipe through `stderr` from the remote machine so it is immediately clear what have happened inside a Codespace
- both "local" and "remote" `--location` options now show "please use sudo" message if user is not root
- `start` command now defaults to "local" if no `--location` option specified, the "auto" option is removed

## 0.3.1

- install `gh net` extension inside a Codespace automatically

## 0.2.0

- add hidden `exists` command. The command simply outputs `yes` if `gh net` extension is installed which can be used to test for extension presence.
- start` command now auto installs or updates `gh net extension on the remote side`
