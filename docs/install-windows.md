# Install Career Agent on Windows

This guide takes you from the download to your first job search. The easiest way is the **Career Agent setup**: one
file that you download and double-click. It brings everything Career Agent needs, its own Python included, so you do
not install anything else first. It takes about five minutes. Windows 10 and Windows 11 (64-bit) are supported.

**You need**

- An internet connection and about 1 GB of free disk space.
- An **OpenAI API key** for the job search (you can add it later). Create one at
  <https://platform.openai.com/api-keys>. OpenAI charges by use, so set a monthly limit in your OpenAI account.

No administrator rights are needed: Career Agent is installed for your Windows account only.

## 1. Download the setup

Open the [Career Agent 1.0.0 release](https://github.com/pistio/career-agent-downloads/releases/tag/v1.0.0) and download
**`CareerAgent-Setup-1.0.0.exe`**. If your browser asks whether to keep the file, choose **Keep**.

## 2. Run the setup

1. Double-click the file in your Downloads folder.
2. If Windows shows **"Windows protected your PC"**, click **More info**, then **Run anyway**. Career Agent's setup is
   not signed with a certificate that Windows knows, so Windows asks the first time. (If the release says the setup is
   signed, this warning does not appear.)
3. The setup speaks your Windows language when it is English, German, Korean, French, Spanish or Portuguese; otherwise
   it asks which language to use.
4. **Select Additional Tasks.** Tick what you want:
   - **Create a desktop shortcut**;
   - **Search for new jobs every Thursday at 07:00 (while you are logged in)**: ticked unless you turned it off
     before;
   - **Add PDF support now (downloads about 150 MB)**: lets Career Agent save your CVs and letters as PDF files. You
     can also add it later (step 6).
5. **Your OpenAI API key.** Paste your key into the field (right-click, Paste, or Ctrl+V). It is hidden while you
   type. You can leave it empty and add the key later by running the setup again. If a key is already stored, leave
   the field empty to keep it. The key is stored in your private settings file, never in the program folder and never
   in a log.
6. Click **Install**, then **Finish**. With **Launch Career Agent** ticked, Career Agent starts at once.

## 3. Start Career Agent

Open the Start menu and click **Career Agent** (or double-click the desktop shortcut). A window titled "Career Agent"
opens and shows:

```text
Career Agent is running at http://127.0.0.1:6300/
Keep this window open while you use Career Agent; close it to stop. (Ctrl+C)
```

Your web browser opens Career Agent. If it does not, copy the address from the window into your browser. When you are
done, close the window. If Windows Firewall asks about Python, you can click Cancel: Career Agent only listens on your
own computer.

The first time, Career Agent opens with a fictional **example profile**, so every screen has something to show.
The [user guide](manual.en.md) explains each screen. Start by filling in your own profile, then run your first search:
**Jobs → Searches → Find opportunities**.

## 4. The weekly search

If you ticked it, Task Scheduler runs **Career Agent weekly search** every Thursday at 07:00 while you are logged in.
If the computer was off or asleep then, the search runs as soon as it can. No window appears. Each search is recorded
in `%LOCALAPPDATA%\Career Agent\logs\weekly.log`.

To turn it on or off later, run the setup again and tick or untick the box. To pick another day or time, change the
task in the Task Scheduler app (its name is "Career Agent weekly search").

## 5. Updating

Close Career Agent first (close its window). Download the new `CareerAgent-Setup-<version>.exe` and run it: it
installs over the old version and keeps your choices, your key and **all your data**.

## 6. PDF support later

Start menu → Career Agent → **Add PDF support to Career Agent**. A window shows the download (about 150 MB) and says
when PDF support is ready. Chromium, the browser engine that makes the PDF files, is kept in
`%LOCALAPPDATA%\ms-playwright`.

## 7. Uninstalling

Open **Settings → Apps → Installed apps**, find **Career Agent**, click **⋯** (or the app) and choose **Uninstall**. Or
use Start menu → Career Agent → **Uninstall Career Agent**. This removes the program, its shortcuts and the weekly
search. **Your data is kept**: the uninstaller says where it is. Delete that folder yourself if you no longer need it.
`%LOCALAPPDATA%\ms-playwright` (PDF support) is not removed either; you can delete it too.

## Where everything is

| What | Where |
| --- | --- |
| The program (with its own Python) | `%LOCALAPPDATA%\Programs\Career Agent` |
| Your data: profiles, searches, shortlist, applications, `settings.env` (your keys; private to your Windows account), `logs\` | `%LOCALAPPDATA%\Career Agent` |

Type either path into the Explorer address bar to open it. [Backup](backup.md) explains how to keep a copy of your
data.

## Troubleshooting (setup)

**"Windows protected your PC" and no "Run anyway" button.** Click **More info** first. On a computer managed by an
organisation, unsigned programs may be blocked for everyone; ask your IT department, or use the script installer below.

**Your antivirus program removes or blocks the setup.** Some antivirus programs distrust new unsigned setups. Check the
file against `SHA256SUMS` on the Releases page (`Get-FileHash .\CareerAgent-Setup-1.0.0.exe` in PowerShell), then allow
it in the antivirus program.

**Career Agent does not start.** Its window stays open and shows why; press a key to close it. The search log is in
`%LOCALAPPDATA%\Career Agent\logs\`.

**The search says the key is missing** (in the weekly log: `the search key OPENAI_API_KEY is not set`). Run the setup
again and paste the key on the key page.

**Silent installation (administrators).** `CareerAgent-Setup-1.0.0.exe /VERYSILENT /SUPPRESSMSGBOXES
/TASKS="weekly,desktopicon" /OPENAIKEYFILE=C:\path\key.txt` installs without questions; the key is read from the first
line of that file (never pass a key on a command line). Uninstall silently with
`"%LOCALAPPDATA%\Programs\Career Agent\unins000.exe" /VERYSILENT`.

---

## Other way: the script installer

The release also has **`career-agent-<version>-windows.zip`**, the program with an installer script that uses a Python
you install yourself. Use it if you prefer to keep Career Agent in a folder of your choice, or if your computer does not
allow the setup. Do not install both ways at once: they use the same data folder and the same weekly task, but each
has its own shortcuts.

**You need** Python 3.12 or newer from python.org (step S1), an internet connection and about 1 GB of free disk space.
It takes about 15 minutes.

### S1. Install Python (once)

1. Go to <https://www.python.org/downloads/windows/> and download the latest **Windows installer (64-bit)**
   (Python 3.13 or newer).
2. Open it. In the first window, **tick "Add python.exe to PATH"**, then click **Install Now**.

Or, in PowerShell: `winget install Python.Python.3.13`

Please do not use the Python from the Microsoft Store: it keeps Career Agent's data in a hidden folder.

### S2. Download and unpack Career Agent

1. Open the **Releases** page of the Career Agent repository on GitHub (the link is on the right of the repository's
   main page) and download the file whose name ends in **`-windows.zip`**, for example
   `career-agent-1.0.0-windows.zip`. Do not download "Source code": it has no ready-made web interface.
2. In your Downloads folder, right-click the zip file and choose **Extract All**, then **Extract**. Do not run anything
   from inside the zip file without extracting it.
3. Move the extracted folder (for example `career-agent-1.0.0`) to a place where it can stay, for example your user
   folder `C:\Users\<your name>\`. The shortcuts and the weekly search point to this folder. Avoid folders that
   OneDrive synchronises (often Desktop and Documents): Career Agent's program files would be uploaded too.

### S3. Run the installer

1. Open the Career Agent folder and double-click **install.cmd**.
2. If Windows shows **"Windows protected your PC"**, click **More info**, then **Run anyway**. If it shows
   **"Open File – Security Warning"**, click **Run**. Career Agent is not signed by Microsoft, so Windows asks once.
3. Answer the questions. Pressing Enter accepts the answer in capitals (`[Y/n]` means Enter is "yes"):

   ```text
   Add PDF support? Career Agent can then save your CVs and letters as PDF files. This downloads about 150 MB. [Y/n]
   Add a “Career Agent” shortcut to the Start menu and the desktop? [Y/n]
   Search for new jobs automatically, every Thursday at 07:00? It runs while you are logged in to this computer; a missed search runs later or the next week. [Y/n]
   Career Agent uses OpenAI to search the web for jobs, so it needs an OpenAI API key (OpenAI charges by use; set a monthly limit there).
   You can create a key here: https://platform.openai.com/api-keys
   Paste your OpenAI API key and press Enter (it stays hidden; just press Enter to add it later):
   ```

   To paste the key, right-click in the window (or press Ctrl+V). The key is not shown. That is normal.

4. The installer then works on its own and shows each step, for example
   `Step 2 of 7: installing the parts Career Agent needs (a few minutes; this needs the internet)` (how many steps
   depends on your answers). Dots show that it is still working.
5. At the end it says **Career Agent is installed.**, shows where your data is kept, and asks
   `Start Career Agent now? [Y/n]`. The window stays open until you press Enter
   (`Press Enter to close this window`).

The messages are in your Windows language when it is English, German, Korean, French, Spanish or Portuguese.

**Other ways to start the installer.** Right-click **install.ps1** and choose **Run with PowerShell** (on Windows 11
under "Show more options"). Or open PowerShell in the folder and run:

```text
powershell -ExecutionPolicy Bypass -File .\install.ps1
```

`-ExecutionPolicy Bypass` lets this one script run this one time; it does not change your computer's settings.
`install.cmd` does exactly the same.

### S4. Start Career Agent

Double-click **Career Agent** on the desktop or in the Start menu. A window titled "Career Agent" opens and shows:

```text
Career Agent is running at http://127.0.0.1:6300/
Keep this window open while you use Career Agent; close it to stop. (Ctrl+C)
```

Your web browser opens Career Agent. If it does not, copy the address from the window into your browser. When you are
done, close the window. If Windows Firewall asks about Python, you can click Cancel: Career Agent only listens on your
own computer.

Without the shortcuts, double-click **start.cmd** in the Career Agent folder.

### S5. First steps

The first time, Career Agent opens with a fictional **example profile**, so every screen has something to show.
The [user guide](manual.en.md) explains each screen. Start by filling in your own profile, then run your first search:
**Jobs → Searches → Find opportunities**.

### S6. The weekly search

If you said yes, Task Scheduler runs **Career Agent weekly search** every Thursday at 07:00 while you are logged in. If
the computer was off or asleep then, the search runs as soon as it can. A window may appear while the search runs; it
closes by itself. Each search is recorded in `%LOCALAPPDATA%\Career Agent\logs\weekly.log`.

- **Another day or time:** run the installer again from PowerShell with, for example,
  `powershell -ExecutionPolicy Bypass -File .\install.ps1 -ScheduleDay mon -ScheduleTime 18:30`.
- **Stop it:** run the uninstaller (step S8), or delete the task in the Task Scheduler app.

### S7. Updating

Close Career Agent first (close its window). Then download the new version, extract it, and run its **install.cmd**.
The shortcuts and the weekly search then point to the new folder, and you can delete the old one. **Your data stays
where it is**: it is never inside the program folder.

### S8. Uninstalling

Double-click **uninstall.cmd** in the Career Agent folder. It removes the shortcuts and the weekly search. **Your data
is kept**; the uninstaller shows where. To remove the program, delete the Career Agent folder afterwards.

To delete your data too, run in PowerShell, in the Career Agent folder:
`powershell -ExecutionPolicy Bypass -File .\uninstall.ps1 -RemoveData`. It lists what will be deleted and asks you to
type `DELETE` (the word is shown in your language). Anything else keeps your data.

### Where your data is

`%LOCALAPPDATA%\Career Agent` (type it in the Explorer address bar): your profiles, searches, shortlist, applications,
`settings.env` (your keys; private to your Windows account) and `logs\`. To see the exact folders, run
`.venv\Scripts\python -m career_agent settings path` in PowerShell in the Career Agent folder. [Backup](backup.md)
explains how to keep a copy.

### Options

`install.ps1` (and `install.cmd`) accept: `-Yes` (ask nothing: PDF support, shortcuts and weekly search yes, no key),
`-NoPdf`, `-NoShortcut`, `-NoSchedule`, `-NoKey`, `-NoStart`, `-ScheduleDay mon|tue|wed|thu|fri|sat|sun`,
`-ScheduleTime HH:MM`, `-Lang en|de|ko|fr|es|pt-PT|pt-BR` and `-WhatIf` (change nothing; show every step). Running the
installer again is always safe: it updates everything in place and never deletes your data.

### Troubleshooting

**"Career Agent needs Python 3.12 or newer, and this computer does not have it (or it is older)."**
Install Python from python.org (step S1; tick "Add python.exe to PATH"), then run the installer again. If you installed
Python while a PowerShell window was open, open a new one.

**"Python was not found; run without arguments to install from the Microsoft Store".**
This is Windows' placeholder, not Python. Install Python from python.org (step S1).

**"running scripts is disabled on this system".**
You started `install.ps1` directly. Double-click `install.cmd` instead, or use the
`powershell -ExecutionPolicy Bypass -File .\install.ps1` line from step S3. On a computer managed by an organisation,
scripts may be blocked for everyone; ask your IT department.

**"The parts could not be installed."**
The installer could not download Career Agent's parts. Check the internet connection and run the installer again. The
details are in `install.log` in the Career Agent folder. Antivirus software sometimes blocks the download; allow it
and try again.

**"PDF support could not be added now; everything else works."**
Run the two commands it shows in PowerShell, in the Career Agent folder, when the internet connection is working.

**Career Agent does not start.**
If Career Agent cannot start, its window stays open and shows why; press a key to close it. If no window opens at all,
the Career Agent folder was probably moved or renamed: run `install.cmd` again in its new place.

**The search says the key is missing** (in the weekly log: `the search key OPENAI_API_KEY is not set`).
Run the installer again and paste the key when asked, or run
`.venv\Scripts\python -m career_agent settings set OPENAI_API_KEY` in PowerShell in the Career Agent folder.
