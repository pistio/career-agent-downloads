# Install Career Agent on Linux

This guide takes you from the download to your first job search. You type two short commands; the installer does
the rest and asks a few yes-or-no questions. It takes about 15 minutes.

**You need**

- A Linux computer with **Python 3.12 or newer**. Ubuntu 24.04, Debian 13, Fedora 39, Linux Mint 22 and newer
  versions have it. (How to check and fix: [Troubleshooting](#troubleshooting).)
- An internet connection and about 1 GB of free disk space.
- An **OpenAI API key** for the job search (you can add it later). Create one at
  <https://platform.openai.com/api-keys>. OpenAI charges by use, so set a monthly limit in your OpenAI account.

## 1. Download

Open the [Career Agent 1.0.0 release](https://github.com/pistio/career-agent-downloads/releases/tag/v1.0.0) and download
**`career-agent-1.0.0-linux.tar.gz`**. Do not download "Source code": it has no ready-made web interface.

## 2. Unpack

1. Open your Downloads folder in the Files app.
2. Right-click the file and choose **Extract Here** (or **Extract**). A folder such as `career-agent-1.0.0` appears.
3. Move that folder to a place where it can stay, for example your **home folder**. The shortcut and the weekly
   search point to this folder, so do not delete it while you use Career Agent.

In a terminal, the same is: `tar xzf ~/Downloads/career-agent-1.0.0-linux.tar.gz -C ~`

## 3. Run the installer

1. Open the folder, right-click an empty part of it and choose **Open in Terminal**. (Or open a terminal and type
   `cd ~/career-agent-1.0.0`.)
2. Type this and press Enter:

   ```text
   sh install.sh
   ```

3. Answer the questions. Pressing Enter accepts the answer in capitals (`[Y/n]` means Enter is "yes"):

   ```text
   Add PDF support? Career Agent can then save your CVs and letters as PDF files. This downloads about 150 MB. [Y/n]
   Add a “Career Agent” shortcut to your applications menu? [Y/n]
   Search for new jobs automatically, every Thursday at 07:00? It runs while you are logged in to this computer; a missed search runs later or the next week. [Y/n]
   Career Agent uses OpenAI to search the web for jobs, so it needs an OpenAI API key (OpenAI charges by use; set a monthly limit there).
   You can create a key here: https://platform.openai.com/api-keys
   Paste your OpenAI API key and press Enter (it stays hidden; just press Enter to add it later):
   ```

   The key is not shown while you paste it. That is normal.

4. The installer then works on its own and shows each step, for example
   `Step 2 of 7: installing the parts Career Agent needs (a few minutes; this needs the internet)` (how many steps
   depends on your answers). Dots show that it is still working.
5. At the end it says **Career Agent is installed.**, shows where your data is kept, and asks
   `Start Career Agent now? [Y/n]`.

The messages are in your computer's language when it is English, German, Korean, French, Spanish or Portuguese.

## 4. Start Career Agent

Open **Career Agent** from your applications menu (search for "Career Agent"). A terminal window opens and shows:

```text
Career Agent is running at http://127.0.0.1:6300/
Keep this window open while you use Career Agent; close it to stop. (Ctrl+C)
```

Your web browser opens Career Agent. If it does not, copy the address from the window into your browser. When you are
done, close the window.

Without the shortcut, run `~/career-agent-1.0.0/start.sh` (the installer prints the exact path).

## 5. First steps

The first time, Career Agent opens with a fictional **example profile**, so every screen has something to show.
The [user guide](manual.en.md) explains each screen. Start by filling in your own profile, then run your first search:
**Jobs → Searches → Find opportunities**.

## 6. The weekly search

If you said yes, Career Agent searches for new jobs every Thursday at 07:00 while you are logged in. If the computer
was off at that time, the search runs the next time you log in. Each search is recorded in
`~/.local/share/career-agent/logs/weekly.log`.

- **Another day or time:** run the installer again with, for example, `sh install.sh --schedule-day mon --schedule-time 18:30`.
- **Stop it:** run the uninstaller (step 8), or `systemctl --user disable --now career-agent-weekly.timer`.

The installer uses a systemd user timer; on a computer without systemd it adds one line to your crontab instead.

## 7. Updating

Close Career Agent first (close its window). Then download the new version, unpack it, and run its installer
(`sh install.sh` in the new folder). The shortcut and the weekly search then point to the new folder, and you can
delete the old one. **Your data stays where it is**: it is never inside the program folder.

## 8. Uninstalling

In the Career Agent folder, run:

```text
sh uninstall.sh
```

It removes the shortcut and the weekly search. **Your data is kept**; the uninstaller shows where. To remove the
program, delete the Career Agent folder afterwards.

To delete your data too, run `sh uninstall.sh --remove-data`. It lists what will be deleted and asks you to type
`DELETE` (the word is shown in your language). Anything else keeps your data.

## Where your data is

`~/.local/share/career-agent` (or `$XDG_DATA_HOME/career-agent`): your profiles, searches, shortlist, applications,
`settings.env` (your keys; private to you) and `logs/`. To see the exact folders, run
`.venv/bin/python -m career_agent settings path` in the Career Agent folder. [Backup](backup.md) explains how to keep a
copy.

## Options

`sh install.sh` accepts: `--yes` (ask nothing: PDF support, shortcut and weekly search yes, no key), `--no-pdf`,
`--no-shortcut`, `--no-schedule`, `--no-key`, `--no-start`, `--schedule-day mon|tue|wed|thu|fri|sat|sun`,
`--schedule-time HH:MM`, `--lang en|de|ko|fr|es|pt-PT|pt-BR` and `--dry-run` (change nothing; show every step).
Running the installer again is always safe: it updates everything in place and never deletes your data.

## Troubleshooting

**"Career Agent needs Python 3.12 or newer, and this computer does not have it (or it is older)."**
Install Python with your system's software tool and run the installer again: on Ubuntu or Debian
`sudo apt install python3 python3-venv`, on Fedora `sudo dnf install python3`, on Arch `sudo pacman -S python`.
`python3 --version` shows what you have. Ubuntu 22.04 and older only have an older Python: upgrade the system, or
install a newer Python as your distribution recommends.

**"Python is installed, but its part for making Career Agent's own Python (venv) is missing."**
Run the command it shows (`sudo apt install python3-venv`, or for example `sudo apt install python3.12-venv`), then
run the installer again.

**"The parts could not be installed."**
The installer could not download Career Agent's parts. Check the internet connection and run the installer again. The
details are in `install.log` in the Career Agent folder.

**"PDF support is installed, but its browser cannot start on this computer."**
The PDF browser needs a few system libraries. Run the command the installer shows (it starts with `sudo` and ends in
`playwright install-deps chromium`), then try a PDF again. Everything else works without it.

**Nothing happens when I click the shortcut.**
The shortcut opens a terminal window; some desktops need a default terminal for that. Run `start.sh` in the Career
Agent folder instead. If you moved or renamed the Career Agent folder, run `sh install.sh` again in its new place.

**The search says the key is missing** (in the weekly log: `the search key OPENAI_API_KEY is not set`).
Run the installer again and paste the key when asked, or run
`.venv/bin/python -m career_agent settings set OPENAI_API_KEY` in the Career Agent folder.

**The address is not 6300.** Another program uses port 6300, so Career Agent picked a free port. Use the address the
window shows.
