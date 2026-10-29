# Install Career Agent on macOS

This guide takes you from the download to your first job search. The easiest way is the **Career Agent package**: one
file that you download and double-click. It installs the **Career Agent** app with everything it needs, its own Python
included, so you do not install anything else first. It takes about five minutes. macOS 11 (Big Sur) or newer.

**You need**

- An internet connection and about 1 GB of free disk space.
- An **OpenAI API key** for the job search (you can add it later). Create one at
  <https://platform.openai.com/api-keys>. OpenAI charges by use, so set a monthly limit in your OpenAI account.
- Your Mac's password once, to install into the Applications folder.

## 1. Download the package

Open the [Career Agent 1.0.0 release](https://github.com/pistio/career-agent-downloads/releases/tag/v1.0.0) and download
the package for your Mac:

- **`CareerAgent-1.0.0-arm64.pkg`** for a Mac with Apple silicon (M1, M2, M3, M4 and later);
- **`CareerAgent-1.0.0-x86_64.pkg`** for a Mac with an Intel processor.

Not sure? Apple menu → **About This Mac**: "Chip: Apple M…" means Apple silicon, "Processor: … Intel …" means Intel.
The wrong package says that it cannot be installed on this computer.

## 2. Install it

1. Double-click the package in your Downloads folder.
2. If macOS says **"CareerAgent-….pkg" cannot be opened because it is from an unidentified developer** (or "Apple
   could not verify…"), click **OK** (or **Done**). Career Agent's package is not signed with an Apple certificate, so
   macOS asks the first time. Then open it anyway, in either way:
   - In Finder, **right-click** (or Control-click) the package, choose **Open**, then **Open** again; or
   - open **System Settings → Privacy & Security**, scroll down to the message about the package, click
     **Open Anyway**, enter your password, then **Open**.

   (If the release says the package is signed and notarised, this warning does not appear.)
3. Click **Continue** through the installer, then **Install**, and enter your Mac's password. Career Agent is
   installed in your **Applications** folder. Click **Close**; you may move the package to the Bin.

## 3. Start Career Agent

Open **Career Agent** from the Applications folder or Launchpad. Drag it to the Dock to keep it handy.

The first time, Career Agent asks three questions (in your Mac's language when it is English, German, Korean, French,
Spanish or Portuguese):

1. **Your OpenAI API key.** Paste it (Cmd+V) and click **Save**; it is hidden while you type. **Later** skips it:
   Career Agent then asks again the next time you open it.
2. **Search for new jobs automatically every Thursday at 07:00?** (step 4)
3. **Add PDF support?** Career Agent can then save your CVs and letters as PDF files. This downloads about 150 MB in
   the background.

Then your web browser opens Career Agent. Career Agent shows its icon in the Dock while it runs; clicking the icon
opens it in the browser again. **To stop Career Agent, quit it like any other app**: right-click its Dock icon and
choose **Quit** (or press Cmd+Q while it is the active app).

The first time, Career Agent opens with a fictional **example profile**, so every screen has something to show.
The [user guide](manual.en.md) explains each screen. Start by filling in your own profile, then run your first search:
**Jobs → Searches → Find opportunities**.

## 4. The weekly search

If you said yes, Career Agent searches for new jobs every Thursday at 07:00, also when the app is not open. If the Mac
is asleep then, the search runs when it wakes up; if it is switched off, the search waits until the next week. Each
search is recorded in `~/Library/Application Support/Career Agent/logs/weekly.log`. It is a LaunchAgent:
`~/Library/LaunchAgents/com.career-agent.weekly.plist`.

To turn it off, open Terminal and run:

```text
"/Applications/Career Agent.app/Contents/Resources/python/bin/python3" -m career_agent native schedule off
```

(`schedule on` turns it on again.)

## 5. Updating

Quit Career Agent. Download the new package and install it the same way: it replaces the old app and keeps your
answers, your key and **all your data**.

## 6. Uninstalling

1. Turn off the weekly search (step 4) if you turned it on.
2. Quit Career Agent and drag **Career Agent** from the Applications folder to the Bin.

**Your data is kept** in `~/Library/Application Support/Career Agent`. Delete that folder yourself if you no longer
need it (in Finder: Go → Go to Folder…, then paste the path). PDF support keeps Chromium in
`~/Library/Caches/ms-playwright`; you can delete that folder too.

## Where everything is

| What | Where |
| --- | --- |
| The app (with its own Python) | `/Applications/Career Agent.app` |
| Your data: profiles, searches, shortlist, applications, `settings.env` (your keys; private to you), `logs/` | `~/Library/Application Support/Career Agent` |

`logs/app.log` there records what Career Agent did while the app was open. [Backup](backup.md) explains how to keep a
copy of your data.

## Troubleshooting (package)

**The package "can't be opened because Apple cannot check it for malicious software", and there is no Open button.**
Use System Settings → Privacy & Security → **Open Anyway** (step 2). It appears there for about an hour after you tried
to open the package.

**"Career Agent could not start. The details are in this file: …"** The last lines of that file (`logs/app.log`) say
why. Quit Career Agent and open it again; if it still fails, install the package again.

**"Career Agent stopped unexpectedly."** The server behind the app stopped; the details are in `logs/app.log`. Open
Career Agent again.

**The search says the key is missing** (in the weekly log: `the search key OPENAI_API_KEY is not set`). Open Career
Agent: it asks for the key while none is stored.

**PDF support did not finish.** Open Terminal and run
`"/Applications/Career Agent.app/Contents/Resources/python/bin/python3" -m career_agent native pdf`; it shows the
download and says when it is ready.

---

## Other way: the script installer

The release also has **`career-agent-<version>-macos.zip`**, the program with an installer script that uses a Python
you install yourself and starts Career Agent in a Terminal window. Use it if you prefer that. Do not install both ways
at once: they use the same data folder and the same weekly search.

**You need** Python 3.12 or newer from python.org or Homebrew (step S1; the `python3` that comes with macOS is too
old), an internet connection and about 1 GB of free disk space. It takes about 15 minutes.

### S1. Install Python (once)

1. Go to <https://www.python.org/downloads/macos/> and download the latest **macOS 64-bit universal2 installer**
   (Python 3.13 or newer).
2. Open the downloaded `.pkg` file and click through the installer.

If you use Homebrew, `brew install python@3.13` works too.

### S2. Download and unpack Career Agent

1. Open the **Releases** page of the Career Agent repository on GitHub (the link is on the right of the repository's
   main page) and download the file whose name ends in **`-macos.zip`**, for example `career-agent-1.0.0-macos.zip`.
   Do not download "Source code": it has no ready-made web interface.
2. Safari unpacks it by itself. Otherwise double-click the zip file in Downloads. A folder such as
   `career-agent-1.0.0` appears.
3. **Move that folder into your home folder** (in Finder: Go, then Home). Not into Desktop, Documents or Downloads:
   macOS protects those folders, and the weekly search would not be allowed to read Career Agent there. If you
   forget, the installer tells you:
   `This folder is inside …/Downloads, which macOS protects: the weekly search would not be allowed to read it. …`

### S3. Run the installer

1. Open **Terminal** (press Cmd+Space, type `Terminal`, press Return).
2. Type `sh` and a space. Then drag the file **install.sh** from the Career Agent folder into the Terminal window and
   press Return. The line looks like this:

   ```text
   sh /Users/you/career-agent-1.0.0/install.sh
   ```

   Running it this way, macOS does not need to check the file, so there is no "unidentified developer" warning. Do not
   double-click `install.sh`: macOS would open it in a text editor or refuse it.

3. Answer the questions. Pressing Return accepts the answer in capitals (`[Y/n]` means Return is "yes"):

   ```text
   Add PDF support? Career Agent can then save your CVs and letters as PDF files. This downloads about 150 MB. [Y/n]
   Add a “Career Agent” shortcut to the Applications folder in your home folder? [Y/n]
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

The messages are in your Mac's language when it is English, German, Korean, French, Spanish or Portuguese.

### S4. Start Career Agent

In Finder, choose Go, then Home, open **Applications** and double-click **Career Agent.command**. (Drag it to the Dock
to keep it handy.) A Terminal window opens and shows:

```text
Career Agent is running at http://127.0.0.1:6300/
Keep this window open while you use Career Agent; close it to stop. (Ctrl+C)
```

Your web browser opens Career Agent. If it does not, copy the address from the window into your browser. When you are
done, close the Terminal window.

Without the shortcut, run `~/career-agent-1.0.0/start.sh` in Terminal (the installer prints the exact path).

### S5. First steps

The first time, Career Agent opens with a fictional **example profile**, so every screen has something to show.
The [user guide](manual.en.md) explains each screen. Start by filling in your own profile, then run your first search:
**Jobs → Searches → Find opportunities**.

### S6. The weekly search

If you said yes, Career Agent searches for new jobs every Thursday at 07:00. If the Mac is asleep then, the search runs
when it wakes up; if it is switched off, the search waits until the next week. Each search is recorded in
`~/Library/Application Support/Career Agent/logs/weekly.log`.

- **Another day or time:** run the installer again with, for example,
  `sh ~/career-agent-1.0.0/install.sh --schedule-day mon --schedule-time 18:30`.
- **Stop it:** run the uninstaller (step S8).

The installer uses a LaunchAgent: `~/Library/LaunchAgents/com.career-agent.weekly.plist`.

### S7. Updating

Close Career Agent first (close its window). Then download the new version, unpack it, move it into your home folder
and run its installer. The shortcut and the weekly search then point to the new folder, and you can delete the old
one. **Your data stays where it is**: it is never inside the program folder.

### S8. Uninstalling

In Terminal, type `sh` and a space, drag **uninstall.sh** from the Career Agent folder into the window and press
Return. It removes the shortcut and the weekly search. **Your data is kept**; the uninstaller shows where. To remove
the program, move the Career Agent folder to the Bin afterwards.

To delete your data too, add `--remove-data` at the end of the line. It lists what will be deleted and asks you to type
`DELETE` (the word is shown in your language). Anything else keeps your data.

### Where your data is

`~/Library/Application Support/Career Agent`: your profiles, searches, shortlist, applications, `settings.env` (your
keys; private to you) and `logs/`. To see the exact folders, run `.venv/bin/python -m career_agent settings path` in the
Career Agent folder. [Backup](backup.md) explains how to keep a copy.

### Options

`sh install.sh` accepts: `--yes` (ask nothing: PDF support, shortcut and weekly search yes, no key), `--no-pdf`,
`--no-shortcut`, `--no-schedule`, `--no-key`, `--no-start`, `--schedule-day mon|tue|wed|thu|fri|sat|sun`,
`--schedule-time HH:MM`, `--lang en|de|ko|fr|es|pt-PT|pt-BR` and `--dry-run` (change nothing; show every step).
Running the installer again is always safe: it updates everything in place and never deletes your data.

### Troubleshooting

**"Career Agent needs Python 3.12 or newer, and this computer does not have it (or it is older)."**
Install Python from python.org (step S1), then **open a new Terminal window** and run the installer again.

**A window asks to install "command line developer tools".**
Something ran the old `python3` that comes with macOS. Click Cancel, install Python from python.org (step S1), open a new
Terminal window and run the installer again.

**macOS says a file "cannot be opened because it is from an unidentified developer" or "Apple could not verify" it.**
Career Agent is not signed by Apple. Run the installer with `sh` in Terminal as in step S3, which needs no approval. If
the **Career Agent** shortcut is refused, remove the download mark from the folder once: in Terminal run
`xattr -dr com.apple.quarantine ~/career-agent-1.0.0` (with your folder's name), then try again.

**"The parts could not be installed."**
The installer could not download Career Agent's parts. Check the internet connection and run the installer again. The
details are in `install.log` in the Career Agent folder.

**The weekly log says "Operation not permitted".**
The Career Agent folder is in Desktop, Documents, Downloads or iCloud Drive, which macOS protects. Move it into your
home folder and run the installer again from there.

**The search says the key is missing** (in the weekly log: `the search key OPENAI_API_KEY is not set`).
Run the installer again and paste the key when asked, or run
`.venv/bin/python -m career_agent settings set OPENAI_API_KEY` in the Career Agent folder.

**The address is not 6300.** Another program uses port 6300, so Career Agent picked a free port. Use the address the
window shows.
