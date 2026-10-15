# PikPak CLI for Linux

A community guide to installing and using the official PikPak CLI on Linux.

Official website: https://mypikpak.com/en-US/cli

## Referral disclosure

The optional registration command below includes invitation code **56485809**.
The author may earn commission when an eligible referred user purchases Premium.
You may register without this code. Existing users should sign in to their existing account.

This is an independent community guide, not an official PikPak project.

## Install

The official installer supports Linux AMD64 and ARM64.

On Debian or Ubuntu, install prerequisites if needed:

```bash
sudo apt-get update
sudo apt-get install -y curl ca-certificates
```

Download and run the official installer:

```bash
curl -fsSL https://download.mypikpak.com/cli/install.sh -o /tmp/pikpak-install.sh
bash /tmp/pikpak-install.sh
```

Check the installation:

```bash
~/.local/bin/pikpak version
```

The default installation directory is `~/.local/bin`.
Follow the installer's instructions if you want to add it to your shell's PATH.

## Sign in with an existing account

```bash
~/.local/bin/pikpak auth login
```

Follow the browser authorization instructions.

On a headless server, open the displayed authorization page on another device,
enter the displayed device code, and approve the connection.
Keep the terminal command running until authorization succeeds.

## Register a new account

Optional registration with the disclosed referral code:

```bash
~/.local/bin/pikpak auth register --affiliate 56485809
```

To register without this referral:

```bash
~/.local/bin/pikpak auth register --affiliate=
```

Check the referral information on the registration page before completing signup.

PikPak requires CLI-created accounts to be verified by signing in through
PikPak Web or the app within 72 hours to avoid a temporary lock.

## List your files

```bash
~/.local/bin/pikpak ls
```

## Explore commands

```bash
~/.local/bin/pikpak --help
~/.local/bin/pikpak upload --help
~/.local/bin/pikpak download --help
~/.local/bin/pikpak skill --help
~/.local/bin/pikpak mcp --help
```

## Verification

CLI v0.5.3 was successfully installed in a Debian Linux AMD64 container
on Unraid. Browser authorization and listing the cloud root were tested.

Native Linux installation and ARM64 have not been tested for this guide.

## Keep account data private

Do not upload credentials, the `.pikpak` configuration folder,
device authorization codes, or private account screenshots to this repository.

## References

- Official CLI: https://mypikpak.com/en-US/cli
- Registration help: https://mypikpak.com/en-US/help-center/connected_apps/pikpak_cli/register_account
- Affiliate instructions: https://mypikpak.com/referral/help-center/en-US/8-cli-and-developer-distribution/how-to-use-invitation-code-in-pikpak-cli
