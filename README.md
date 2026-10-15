# PikPak CLI for Docker

Run the official PikPak CLI in a Debian container with persistent login
and a local folder for file transfers.

This is an independent community guide, not an official PikPak project.

Official CLI: https://mypikpak.com/en-US/cli

## Referral disclosure

The optional registration command includes invitation code **56485809**.
The author may earn commission when an eligible referred user purchases Premium.

Existing users should sign in to their existing account.
You can register without the referral code using `--affiliate=`.

## AI agent skill

Install this skill to let your AI agent help you set up and use PikPak CLI in Docker:

```bash
npx skills add dvnhan91/pikpak-docker-guide --skill pikpak-docker
```

Example prompt:

> Help me set up PikPak CLI in Docker with persistent login and a downloads folder.

## Requirements

- Docker with Docker Compose support
- Internet access
- A PikPak account, or a new account created during setup

The official installer supports Linux AMD64 and ARM64.
This guide's manual installation was tested on AMD64 using Unraid.

## 1. Download and start

Download this repository and open a terminal in its directory.

Alternatively, if Git is installed:

```bash
git clone https://github.com/dvnhan91/pikpak-docker-guide.git
cd pikpak-docker-guide
```

Start the container:

```bash
docker compose up -d
docker compose logs -f pikpak-cli
```

On its first start, the container installs dependencies and downloads
PikPak CLI from the official website.

Wait for installation to finish. Press Ctrl+C to exit the log viewer;
the container keeps running.

Check the installation:

```bash
docker compose exec pikpak-cli pikpak version
```

## 2. Sign in

For an existing account:

```bash
docker compose exec pikpak-cli pikpak auth login
```

Follow the authorization instructions shown in the terminal.

On a headless server, open the displayed page on your computer or phone,
enter the displayed device code, and approve the connection.
Keep the terminal command running until authorization succeeds.

Enter your password only on PikPak's website.
Do not share device authorization codes.

### New users: optional referral registration

If you need a new account, register using the disclosed referral code:

```bash
docker compose exec pikpak-cli pikpak auth register --affiliate 56485809
```

To register without a referral code:

```bash
docker compose exec pikpak-cli pikpak auth register --affiliate=
```

Check the referral information on the registration page before completing signup.

According to PikPak's registration instructions, CLI-created accounts
must be verified by signing in through PikPak Web or the app within
72 hours to avoid a temporary lock.

## 3. List cloud files

```bash
docker compose exec pikpak-cli pikpak ls
```

## 4. Transfer files

The local `files` directory is mounted as `/files` inside the container.

Put a local file named `example.txt` in that directory, then upload it:

```bash
docker compose exec pikpak-cli pikpak upload /files/example.txt
```

To download a cloud file, replace `FILE_ID` with its actual ID from the
file listing:

```bash
docker compose exec pikpak-cli pikpak download FILE_ID -o /files/downloaded.txt
```

The downloaded file appears in the local `files` directory.
Choose an output filename that does not overwrite a file you need.

Transfers are subject to your account's storage and usage limits.

## Dockhand and Unraid

Create a stack in Dockhand and paste the contents of `compose.yaml`.

On Unraid, replace the relative volume mappings with persistent host paths:

```yaml
    volumes:
      - /mnt/user/appdata/pikpak-cli/config:/root/.pikpak
      - /mnt/user/pikpak-files:/files
```

Deploy the stack and wait for installation to finish in its logs.

Open the container's Shell, select Bash, then run:

```bash
pikpak version
pikpak auth login
pikpak ls
```

For new-account registration with the optional referral code:

```bash
pikpak auth register --affiliate 56485809
```

### Existing manual installations

If you previously installed the CLI manually into `/root/.local/bin`
and mounted the entire `/root` directory, keep using:

```bash
/root/.local/bin/pikpak ls
```

That existing setup does not need to be replaced to use the CLI.

If migrating to this repository's Compose configuration, first copy the
existing `.pikpak` directory contents into the new host `config` directory.
Keep the original data until the migrated login has been verified.

## Persistent data

| Host directory | Container path | Purpose |
| --- | --- | --- |
| `./config` | `/root/.pikpak` | Authentication and CLI state |
| `./files` | `/files` | Local uploads and downloads |

Keep the same host directories when recreating the container.

The container runs as root, so downloaded files may be root-owned.
No ports are published. This setup provides CLI access, not a web interface
or automatic file synchronization.

The CLI is installed into the container filesystem.
After recreation, the installer downloads the version PikPak currently
provides. Restarting the same container keeps its installed version.

## More commands

```bash
docker compose exec pikpak-cli pikpak --help
docker compose exec pikpak-cli pikpak upload --help
docker compose exec pikpak-cli pikpak download --help
docker compose exec pikpak-cli pikpak skill --help
docker compose exec pikpak-cli pikpak mcp --help
```

Installing an AI skill in this container does not automatically connect
an AI agent running on another computer.

## Troubleshooting

- **Container Running but CLI unavailable:** check logs. Installation may
  still be running or may have failed.
- **Waiting for authorization:** approve the connection on PikPak's page
  while leaving the terminal command running.
- **Login missing after recreation:** check that `/root/.pikpak` maps to
  the same persistent host directory.
- **Downloaded file missing:** use a container output path under `/files`
  and check the corresponding host directory.
- **No logs after installation:** normal; the container waits for CLI commands.

## Verification status

The official CLI v0.5.3 was manually installed in a Debian AMD64 container
on Unraid. Browser authorization and listing the cloud root succeeded.

The automatic bootstrap in this repository's Compose configuration,
ARM64 deployment, and file transfers have not yet been tested end to end.
Upload and download syntax was checked against CLI v0.5.3 help.

## Privacy

Do not upload login credentials, the `config` directory, authorization
codes, or private account screenshots to this public repository.

## Official references

- CLI: https://mypikpak.com/en-US/cli
- Registration: https://mypikpak.com/en-US/help-center/connected_apps/pikpak_cli/register_account
- Referral code usage: https://mypikpak.com/referral/help-center/en-US/8-cli-and-developer-distribution/how-to-use-invitation-code-in-pikpak-cli

Commission depends on valid referrals and eligible Premium purchases.
Installing the CLI or publishing this guide does not itself generate earnings.
