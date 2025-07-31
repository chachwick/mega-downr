# mega-downr

**mega-downr** is a smart Bash script for downloading from Mega.nz on Linux or Unix-based servers. It was specifically created to make it easy to download files from the cloud directly onto a local NAS.It is especially helpful in downloading from shared links that will not be syncronized later. For example, large video files uploaded to Mega.nz by the PAs on a remote shoot can be downloaded to the NAS of the editing team so they can get working locally. 

 It wraps `megacmd` to support:

- `--screen` to run safely in background sessions
- `--verbose` for detailed transfers and speeds
- `--dry-run` to list contents without downloading (for your own Mega folders)
- `--quiet` and `--summary-only` for minimal reports
- `--log <file>` to keep a permanent record of your downloads

This script works on any Linux or Unix system, but was designed with OpenMediaVault (OMV) NAS systems in mind, often running on ProxMox or similar servers. After downloading, you can use `megacmd` tools to handle syncing or other transfers. 

------

## Why use this script?

- You have a local NAS running OpenMediaVault (OMV), an open-source NAS platform commonly deployed on ProxMox servers and other hardware, and need to pull files from Mega.nz.
- You receive download links from clients or colleagues via Mega.nz and want them to go directly to your NAS.
- You want to pull large files from your own Mega.nz cloud storage onto your NAS without tying up your laptop or desktop.
- You need to run long downloads overnight or over the weekend and want them to complete even if your SSH session disconnects.
- You use a laptop that comes and goes, but rely on your OMV or other server to handle heavy downloads in the background.

------

## Install

Clone this repo and make it executable:

```bash
git clone https://github.com/chachwick/mega-downr.git
cd mega-downr
sudo cp mega-downr /usr/local/bin/
sudo chmod +x /usr/local/bin/mega-downr
```

------

## OMV / Debian prerequisites

On OpenMediaVault (or Debian), install:

- `megacmd` (MEGA command line tools)
- `screen` (to run detached sessions)
- `curl`, `wget`, `sudo` (usually pre-installed)

Run:

```bash
sudo apt update
sudo apt install megacmd screen curl wget sudo -y
```

Then start the MEGAcmd daemon (if not running automatically):

```bash
mega-cmd-server &
```

Log into your Mega account if needed:

```bash
mega-login you@example.com YourPassword
```

------

## Using screen for large downloads

For large downloads, use `--screen` so your transfer continues even if you disconnect SSH. This is ideal for overnight or weekend jobs.

Reconnect to the session with:

```bash
screen -r mega-downr-job
```

### Basic screen tips

- Detach: `Ctrl + A` then `D`
- List sessions: `screen -ls`
- Reattach: `screen -r <name or ID>`
- Exit: type `exit` inside the session

------

## Usage examples

| Command                                             | What it does                         |
| --------------------------------------------------- | ------------------------------------ |
| `mega-downr /MyMegaFolder`                          | Downloads your folder to current dir |
| `mega-downr -d /MyMegaFolder`                       | Dry-run list of contents             |
| `mega-downr -s -v "https://mega.nz/folder/XYZ#KEY"` | Runs shared link in detached screen  |
| `mega-downr --log my.log /MyMegaFolder`             | Logs all output to `my.log`          |
| `mega-downr --quiet /MyMegaFolder`                  | Minimal output                       |
| `mega-downr --summary-only /MyMegaFolder`           | Just totals at end                   |

------

## Note on shared links

- The script can download shared links, but `mega-ls` does not work on them. This means `--dry-run` and `--summary-only` will show helpful messages instead.
- For shared links, use `--verbose` and a temporary directory to preview downloads.

------

## License

MIT License — see [LICENSE](LICENSE).

------

## Contributing

PRs and issues welcome. Feel free to fork and improve!

------

## Credits

- Script by [Chadwick Self](https://github.com/chachwick)
- Powered by [MEGAcmd](https://github.com/meganz/MEGAcmd)