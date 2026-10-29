# Backup: keeping your job search safe

## In short

Career Agent's program code is public and kept on GitHub. **Your job search is not.** Your profile and career
history, your searches, the Shortlist and your decisions, and your applications with their CVs and letters stay
only on the computer that runs Career Agent. Nobody else has a copy.

So if that computer breaks, is lost or is wiped, your job search goes with it, unless you have your own copy. Ask
the person who installed Career Agent to set up a regular backup (the steps are below), and keep it somewhere only
you can read, for example an encrypted USB disk. Keep passwords and API keys in a password manager, not in the
backup.

The rest of this page is **for the person who installs it**.

## What is in Git and what is not

Career Agent keeps four kinds of material apart (`docs/architecture/profile-data-separation.md`): the program, the
fictional example, a person's own data, and secrets. Only the program and the example ever go into Git. A GitHub
repository of Career Agent is therefore a backup of **the software**, not of anyone's job search.

| Layer | What | In the Git repository | Backed up by |
| --- | --- | --- | --- |
| Engine | `career_agent/`, `ui/` source, `tests/` (public), `docs/` (public), `deploy/examples/`, `scripts/`, `brand/`, `ui/public/` assets | **yes** | Git / GitHub |
| Example | `profiles/example/`: the fictional Alex Morgan profile | **yes** | Git / GitHub |
| Private profiles | `profiles/<id>/`: career history, CV evidence, job targets, voice guide, contact details | **never** | your own private backup |
| Runtime data | `runs/`, `shortlist/`, `decisions/`, `applications/`, `application-prep/`, `advert-evidence/`, `role-synopses/`, `automation-runs/`, `archives/` | **never** | your own private backup |
| Secrets | API keys and tokens (`.env`, credentials files) | **never** | your password manager |
| Installation | `deploy/systemd/` (the filled-in units of one server), private docs and compatibility tests, `private/` | **never** | your own private backup |

### What GitHub does back up

The program's source code (the engine and the web interface), the public tests, the documentation, the generic
deployment templates, the fictional example profile, the brand images the app needs, and the scripts that build and
check an export. A fresh copy (clone) of it installs and tests on its own (`docs/developer-reference.md`,
"Quick start").

### What GitHub deliberately does not back up

A person's profile, every search and its history, the shortlist and decisions, applications and their
generated CVs and letters, advert evidence, archives of earlier resets, the private compatibility tests,
and every secret. If the machine is lost and there is no separate backup of these, they are gone; the Git
repository cannot give them back.

**Back these up separately and privately**, for example an encrypted copy of `profiles/<id>/` and of the
data folder (the data root: the project folder, or `CAREER_AGENT_DATA_DIR` when set), plus the filled-in
`deploy/systemd/` units. Keep secrets in a password manager, not in a backup of the folders.

## Backing up an installation's data

Copying two folders saves everything private: the **data root** (`CAREER_AGENT_DATA_DIR`; without it, the
checkout itself) and the **profiles root** (`CAREER_AGENT_PROFILES_DIR`; usually `<data root>/profiles`). There is no
database to dump. Secrets are not in either folder; keep them in a password manager.

1. **Pick a quiet moment.** Career Agent saves each file in one step, but a copy taken while a search runs can hold
   only half of that search. Check that nothing is running, as the service user, with the same roots the units use:

   ```bash
   CAREER_AGENT_DATA_DIR=<data root> CAREER_AGENT_PROFILES_DIR=<profiles root> \
     .venv/bin/python -m career_agent.automation status --limit 3
   ```

   No line may say `running`, and the web interface must show no search in progress. (Avoid the weekly timer's hour,
   `systemctl list-timers career-agent-weekly.timer`.)

2. **Copy both roots** to storage that only you can read, for example an encrypted disk:

   ```bash
   # archives, one pair per backup
   tar -czf /backup/career-agent-data-$(date +%Y%m%d).tar.gz -C <data root> --exclude=./.tmp .
   tar -czf /backup/career-agent-profiles-$(date +%Y%m%d).tar.gz -C <profiles root> .
   # or mirrors (fast, incremental)
   rsync -a --delete --exclude=.tmp/ <data root>/ /backup/career-agent-data/
   rsync -a --delete <profiles root>/ /backup/career-agent-profiles/
   ```

   When the profiles root lies inside the data root (`<data root>/profiles`), the data copy already holds it; the
   second command is then not needed. `.tmp/` is scratch space and need not be kept. Also keep the filled-in units and
   environment files without their secrets (or a note of where they came from).

3. **Check now and then that the copy can be restored**: unpack it into an empty private folder and validate each profile there,
   from the checkout:

   ```bash
   mkdir -m 0700 <check>
   mkdir <check>/data <check>/profiles
   tar -xzf /backup/career-agent-data-<date>.tar.gz -C <check>/data
   tar -xzf /backup/career-agent-profiles-<date>.tar.gz -C <check>/profiles   # or use <check>/data/profiles
   CAREER_AGENT_DATA_DIR=<check>/data CAREER_AGENT_PROFILES_DIR=<check>/profiles \
     .venv/bin/python -m career_agent profile validate <id>
   CAREER_AGENT_DATA_DIR=<check>/data .venv/bin/python -m career_agent.automation status --limit 1
   ```

   Every profile must validate and `status` must list the latest searches. Delete `<check>` afterwards: it holds the
   same private data as the backup.

## How the repository is produced (for developers)

The repository is the source: an installation runs a clone of it, with its private profiles and runtime data in
a data root outside the checkout (`docs/development.md`). Two checks keep a person out of it: before every push,
`scripts/prepush_check.py` scans the committed tree with the installation's private terms and runs the publication
test; on GitHub, CI runs the generic privacy scan and the publication test again, which also fails if a tracked
file is a symlink, lies under a runtime folder or a private profile, or is not on the publication allowlist.

The exporter remains for a fresh, scanned copy of the tree (for example a public release):

```bash
python3 scripts/export_public.py <target>
```

1. copies an **allowlist** (engine folders and files, public docs and tests, `profiles/example/`) into a
   fresh staging folder, and nothing else, so a new `profiles/<someone>/` can never ship by accident;
2. scans it with `scripts/privacy_scan.py`: secrets, e-mail addresses, phone numbers, registration
   numbers, private network addresses and hostnames, home paths, private profile folders, and the literal
   private terms of the installation (from `private/*.txt` and each private profile's names), reported as
   file, line and type, never the value;
3. runs `tests/test_publication_surface.py` inside the staged tree, which also asks Git, by its own rules, whether any file
   the export publishes would be silently ignored (a `credentials.*` rule once hid a source file from the backup); an
   exception is a negation in `.gitignore` next to the rule it escapes;
4. only if both are clean, replaces the export's files, keeping its `.git`, so each re-export is an
   ordinary commit.

`.gitignore` is the second line of defence: it ignores every profile except `profiles/example/` (an
allowlist), all runtime folders, secrets, build output and generated PDFs.

## Restoring

- **Software:** clone the repository, then follow the install guide for your computer (README, "Install") or the
  quick start in `docs/developer-reference.md`.
- **A person's search:** restore `profiles/<id>/` and the data folders from the private backup into the
  project (or into the folders `CAREER_AGENT_PROFILES_DIR` and `CAREER_AGENT_DATA_DIR` name), then run
  `python -m career_agent profile validate <id>`.
- **Secrets:** put them back into the environment or credentials file (`.env.example`,
  `deploy/examples/credentials.example`).
