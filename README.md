# RemoLog

**Born out of paranoia.** RemoLog keeps a record macOS doesn't: which program moved,
renamed or deleted a file on your Mac, and who logged in or used `sudo`. When a file
vanishes, the log says what took it.

## What it records

- Files moved, renamed, deleted, copied or linked, and the program that did it. A move
  to the Bin is a move, so it shows up too.
- Changes to a file's owner, permissions or flags.
- Files that iCloud Drive and other cloud folders download or update.
- Logins, logouts, screen locks and unlocks, `sudo` and `su`.
- Apps adding or removing something that runs in the background.

It does not record what is inside your files, files that are only opened or read, or
anything on the network.

## Requirements

- A Mac with Apple silicon (M1 or later). Intel Macs are not supported yet.
- macOS 15 Sequoia or later.

## Install

1. Download `RemoLog-<version>.pkg` from [Releases](https://github.com/okoker/remolog/releases/latest).
2. Open it and follow the installer. It asks for an administrator password.
3. RemoLog opens and asks for two switches:
   - **Full Disk Access**, so it can see file activity. Click Open Settings and switch
     RemoLog on.
   - **Notifications**, so it can tell you if it ever stops. Click Allow.

Then it says Recording. Close the window: RemoLog keeps recording, and starts by itself
after a restart.

## Update

Download the new `RemoLog-<version>.pkg` and open it. Your logs and settings stay;
recording pauses for a few seconds.

## Uninstall

Open RemoLog from Applications and click **Uninstall…**. Your logs stay in
`/var/log/remo_logs` unless you tick "Also delete all logs".

## Your logs

- One file per day (UTC) in `/var/log/remo_logs`, one JSON record per line. Finished
  days are compressed.
- RemoLog keeps 7 days. **Keep logs for**, in its window, changes that to 14, 30 or 90.
- **Open logs in Terminal**, in its window, opens Terminal there. For example, what went
  to the Bin:
  ```sh
  zcat -f /var/log/remo_logs/*.jsonl* | jq -c 'select(.dst? // "" | test("/\\.Trash/"))'
  ```
- Every account on this Mac can read the logs. Only RemoLog writes to them.

## If something goes wrong

RemoLog checks itself every 5 minutes. It sends a notification if it stops recording,
misses events or runs short of disk space. Open RemoLog to see what's wrong; where it
can, it offers to fix it.

## What it can't do

- An administrator can stop it, without a password, by moving RemoLog out of
  Applications. The log keeps a record that it stopped.
- In a flood of file activity, macOS itself can drop events before RemoLog sees them.
  RemoLog counts what was lost and tells you.
- The logs live on the Mac they describe, so anyone with administrator rights can delete
  or change them.

## Privacy

RemoLog sends nothing anywhere. It has no networking code, no accounts and no analytics.

---

© 2026 Objects Consulting Ltd.
