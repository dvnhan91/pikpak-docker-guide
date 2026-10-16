# PikPak CLI for Docker

Run the official PikPak CLI in Docker on Linux, Unraid, and Dockhand with persistent login and a local folder for file transfers.

Use the included AI agent skill for setup, cloud tasks, NAS transfers, and playback URL workflows.

This is an independent community project, not an official PikPak project.

Official CLI: https://mypikpak.com/en-US/cli

## AI agent skill

Install the skill:

```bash
npx skills add dvnhan91/pikpak-docker-guide --skill pikpak-docker
```

[View this skill on skills.sh](https://www.skills.sh/dvnhan91/pikpak-docker-guide/pikpak-docker)

### Workflows covered

- Set up Docker with persistent login and a local transfer folder.
- Save URLs or magnet links to PikPak and track cloud tasks.
- Browse cloud folders and identify files.
- Download cloud files to your NAS.
- Upload local files to PikPak.
- Obtain playback URLs for use outside the container.
- Troubleshoot authentication, Docker paths, and permissions.

The skill guides your agent through the installed CLI's commands. Available features depend on your CLI version and account.

Install the skill where your AI agent runs. Installing it inside the PikPak container does not automatically connect an agent running on another computer. The agent needs authorized terminal or SSH access to operate your Docker host.

### Example prompts

> Set up PikPak CLI in Dockhand with persistent login.

> Save this magnet link to my PikPak folder and check its progress.

> Download this cloud file to my NAS downloads directory.

> Upload this local file from my NAS to PikPak.

> Get a playback URL for episode 3.

> My login disappeared after recreating the container.

## Referral disclosure

The optional registration command includes invitation code **56485809**. The author may earn commission when an eligible referred user purchases Premium.

Existing users should sign in to their existing account.

New users can choose the disclosed referral code, another code, or no code. Use `--affiliate=` to register without a referral code, including when the installer previously recorded one.

Installing the skill does not create an account or submit the referral code.

## Requirements

- Docker with Docker Compose support
- Internet access
- A PikPak account, or a new account created during setup
- An AI agent and Node.js/npm if installing the skill through `npx`

The official installer supports Linux AMD64 and ARM64. The manual CLI installation described below was tested on AMD64 using Unraid.

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

On its first start, the container installs dependencies and downloads PikPak CLI from the official website.

Wait for installation to finish. Press Ctrl+C to exit the log viewer; the container keeps running.

Check the installation:

```bash
docker compose exec pikpak-cli pikpak version
```

These commands assume you are using this repository's Compose configuration.

## 2. Sign in

For an existing account:

```bash
docker compose exec pikpak-cli pikpak auth login
```

Follow the authorization instructions shown in the terminal.

On a headless server, open the displayed page on your computer or phone, enter the displayed device code, and approve the connection.

Keep the terminal command running until authorization succeeds.

Enter your password only on PikPak's website. Do not share device authorization codes.

### New users: optional referral registration

If you choose to register with the author's disclosed referral code:

```bash
docker compose exec pikpak-cli pikpak auth register --affiliate 56485809
```

To register without a referral code:

```bash
docker compose exec pikpak-cli pikpak auth register --affiliate=
```

To use another invitation code, replace `56485809` with your chosen code.

Check the referral information on the registration page before completing signup.

According to PikPak's registration instructions, CLI-created accounts must be verified by signing in through PikPak Web or the app within 72 hours to avoid a temporary lock.

## 3. Browse cloud files

List the cloud root:

```bash
docker compose exec pikpak-cli pikpak ls
```

Inspect the commands available in your installed version:

```bash
docker compose exec pikpak-cli pikpak ls --help
docker compose exec pikpak-cli pikpak get --help
```

Use the IDs returned by the CLI when selecting files or folders. Cloud file IDs are different from local filesystem paths.

## 4. Transfer files

The local `files` directory is mounted as `/files` inside the container.

### Upload a local file

Put a file named `example.txt` in the local `files` directory, then run:

```bash
docker compose exec pikpak-cli pikpak upload /files/example.txt
```

### Download a cloud file

Replace `FILE_ID` with an actual file ID from the cloud listing:

```bash
docker compose exec pikpak-cli pikpak download FILE_ID -o /files/downloaded.txt
```

Choose an appropriate output filename and extension. Avoid overwriting a file you need.

The downloaded file appears in the local `files` directory.

For additional options:

```bash
docker compose exec pikpak-cli pikpak upload --help
docker compose exec pikpak-cli pikpak download --help
```

Transfers are subject to your account's storage and usage limits.

## 5. Cloud URL and magnet tasks

Cloud tasks save content to PikPak's servers. Downloading a cloud file to your NAS is a separate operation.

Inspect the task commands supported by your installed CLI:

```bash
docker compose exec pikpak-cli pikpak task --help
docker compose exec pikpak-cli pikpak task add --help
docker compose exec pikpak-cli pikpak task get --help
```

Use the syntax shown by your installed version to submit a URL or magnet link and query the returned task ID.

Quote URLs and magnet links when entering them in a shell, especially when they contain `&`.

The AI agent skill guides the following workflow:

1. Use the link supplied by the user.
2. Resolve the requested cloud destination.
3. Submit the task and capture its ID.
4. Check whether the task is downloading, completed, or failed.
5. Locate the resulting cloud files after completion.

An accepted task does not mean the cloud download has finished. Avoid repeatedly submitting the same link when a task fails.

Use these workflows for content you are entitled to obtain.

## 6. Playback URLs

Inspect playback options:

```bash
docker compose exec pikpak-cli pikpak play --help
```

If your CLI supports URL-only output, use it to obtain a playback URL for the selected cloud video.

Open the returned URL on your computer or another compatible player. A headless Docker container does not need to launch a graphical player.

Playback URLs may expire and should not be published. A returned URL does not guarantee compatibility with every player.

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

To register without a referral code:

```bash
pikpak auth register --affiliate=
```

### Host paths and container paths

With the Unraid mapping above:

| Location | Path |
| --- | --- |
| File inside the container | `/files/example.mp4` |
| Same file on the Unraid host | `/mnt/user/pikpak-files/example.mp4` |

CLI commands running inside the container use `/files/...`.

If you change the host directory, use your actual volume mapping to locate the corresponding file.

### Existing manual installations

If you previously installed the CLI manually, locate it inside the container:

```bash
command -v pikpak
```

If it is absent from PATH, check common locations:

```bash
ls -l /root/.local/bin/pikpak /usr/local/bin/pikpak
```

Use the executable that exists. For an existing installation under `/root/.local/bin`:

```bash
/root/.local/bin/pikpak version
/root/.local/bin/pikpak ls
```

From the Docker host, if your container is named `pikpak-cli`:

```bash
docker exec pikpak-cli /root/.local/bin/pikpak ls
```

For interactive login from the host:

```bash
docker exec -it pikpak-cli /root/.local/bin/pikpak auth login
```

An existing working setup does not need to be replaced to use the skill.

If migrating to this repository's Compose configuration, first copy the existing `.pikpak` directory contents into the new host `config` directory. Keep the original data until the migrated login has been verified.

## Persistent data

For the default Compose configuration:

| Host directory | Container path | Purpose |
| --- | --- | --- |
| `./config` | `/root/.pikpak` | Authentication and CLI state |
| `./files` | `/files` | Local uploads and downloads |

Keep the same host directories when recreating the container.

The container runs as root, so downloaded files may be root-owned.

No ports are published. This setup provides CLI access, not a web interface or automatic file synchronization.

The CLI is installed into the container filesystem. After recreation, the installer downloads the version PikPak currently provides. Restarting the same container keeps its installed version.

## More commands

Check the installed version's help before using unfamiliar operations:

```bash
docker compose exec pikpak-cli pikpak --help
docker compose exec pikpak-cli pikpak share --help
docker compose exec pikpak-cli pikpak skill --help
docker compose exec pikpak-cli pikpak mcp --help
```

Review the intended files before deleting, overwriting, making public shares, or performing large batch changes.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| Container running but CLI unavailable | Installation logs and startup command; installation may still be running or may have failed. |
| Command not found | PATH and the executable's actual location inside the container. |
| Waiting for authorization | Approve access on PikPak's page while leaving the terminal command running. |
| Login missing after recreation | Confirm `/root/.pikpak` maps to the same persistent host directory and the container user can access it. |
| Downloaded file missing | Check the container output path and its corresponding host volume mapping. |
| Permission denied | Inspect the container user, file ownership, and directory permissions. |
| Cloud task failed | Keep its task ID and inspect the actual error before retrying. |
| No logs after installation | Normal when the container is idle and waiting for CLI commands. |

## Verification status

Verified:

- Manual installation of official PikPak CLI v0.5.3 in a Debian AMD64 container on Unraid.
- Browser authorization and listing the cloud root.
- Detection and installation of the `pikpak-docker` agent skill through the `skills` CLI.
- CLI v0.5.3 help for referral registration and upload/download syntax.

Not yet tested end to end:

- Automatic bootstrap using this repository's Compose configuration.
- ARM64 deployment.
- Upload and download transfers.
- Cloud task submission and monitoring.
- Playback URL workflows.
- The expanded skill's execution of these workflows by an AI agent.

The skill instructs agents to inspect the installed CLI's help and verify results. Successful skill installation alone does not verify every workflow.

## Privacy

Do not upload login credentials, the `config` directory, authorization codes, playback URLs, or private account screenshots to this public repository.

Keep account authorization on PikPak's official website.

## Official references

- CLI: https://mypikpak.com/en-US/cli
- Registration: https://mypikpak.com/en-US/help-center/connected_apps/pikpak_cli/register_account
- Referral code usage: https://mypikpak.com/referral/help-center/en-US/8-cli-and-developer-distribution/how-to-use-invitation-code-in-pikpak-cli

Commission depends on valid referrals and eligible Premium purchases. Installing the CLI or publishing this guide does not itself generate earnings.
