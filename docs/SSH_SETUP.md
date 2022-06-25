## Setting up SSH to connect to Codespaces

> Note: Starting from [GitHub CLI 2.13.0](https://github.com/cli/cli/releases/tag/v2.13.0) the `SSH` config is created automatically for all your Codespaces. You can skip all instructions in this doc if you use the appropriate CLI version. We recommend upgrading; please see [Installation](https://github.com/cli/cli#installation) section of GitHub CLI for the upgrading steps.

The extension relies on `gh codespace ssh` command to establish `SSH` tunnel to a Codespace; hence you need to set up `SSH` keys if you haven't yet. A good way to test the setap is to do `sudo gh codespace ssh` (`sudo` is vital since kernel might use a different identity under a non-root user). If you can connect to a Codespace successfully, you are all set. Refer to [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for more info. A list of known issues and workarounds for them can be found in the [Common issues](#common-issues) section below and in issues marked with the [known-issue label](https://github.com/github/gh-net/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue).

Because `sudo` needs to be used to start the `gh-net` extension, the `SSH` config has to go to the default location for the `root` user, which is `/root/.ssh/` on `Linux` machines. However, the `MacOSX` machines can use `~/.ssh/` as the default location even for the `root` user, so no special config treatment is needed for the Mac machines.


### Common issues

- `SSH` key has a passphrase, and it's impossible to enter it when prompted.

Make sure to add the key to the `SSH` agent before you try to connect to a Codespace. Refer to [adding you key to SSH agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent) section for step-by-step instructions.

- The `Enter password for user@codespace:` prompt pops up, but no password work.

It means that the correct `SSH` key or/and config were not picked up by `SSH` agent. Please refer to the previous section's solution to resolve this.
