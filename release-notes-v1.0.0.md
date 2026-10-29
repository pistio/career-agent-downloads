# Career Agent 1.0.0

Career Agent runs on your own computer and helps you find and assess roles, organise a shortlist, prepare application
documents from your own career history and practise for interviews. You make every decision. It does not submit
applications or contact employers for you.

## System requirements

- **Windows:** Windows 10 or 11, 64-bit. The setup installs for the current user and normally needs no administrator
  rights.
- **macOS:** macOS 11 or newer. Choose the Apple-silicon package for M-series Macs or the Intel package for Intel
  Macs. Installing into Applications requires an administrator password.
- **Linux:** Python 3.12 or newer, an internet connection and standard build tools. Optional PDF support may require
  additional system libraries; the installer reports the command when they are missing.

The Windows and macOS installers bundle Python, so a separate Python installation is not required. Optional PDF
support downloads approximately 150 MB of browser components.

## Costs and privacy

Career Agent is free under the MIT licence and has no Career Agent subscription. Searching and AI-assisted writing
use your own OpenAI API key by default; OpenAI charges separately by use. Set a spending limit in your API account.
Career Agent stores its working data locally, while search and AI features contact the providers you configure.

Career Agent prepares drafts and recommendations. It never applies for a job, sends a CV or contacts an employer on
your behalf.

## Signing and platform coverage

The Windows setup is unsigned. The macOS packages are unsigned and not notarised. Windows or macOS may therefore
show a warning before installation; follow the relevant installation guide.

The native installers were tested on Windows Server 2022, macOS 14 on Apple silicon and macOS 15 on Intel. Windows 10
and 11 were not tested directly. macOS install and launch were tested on both architectures, but upgrade and uninstall
coverage was more complete on Windows. The Linux archive was tested on Ubuntu 24.04 with Python 3.12 and 3.14; other
Linux distributions were not independently tested.

Tests covered installation, startup, local data placement, shortcuts, scheduling, private API-key storage and PDF
creation. No job search, paid AI inference, application submission or Telegram message was performed during release
testing.

## Downloads and checksums

Download the installer for your platform from the
[public v1.0.0 release](https://github.com/pistio/career-agent-downloads/releases/tag/v1.0.0). Verify it with the
release's `SHA256SUMS` file before running it.

- [Install on Windows](https://github.com/pistio/career-agent-downloads/blob/main/docs/install-windows.md)
- [Install on macOS](https://github.com/pistio/career-agent-downloads/blob/main/docs/install-macos.md)
- [Install on Linux](https://github.com/pistio/career-agent-downloads/blob/main/docs/install-linux.md)
