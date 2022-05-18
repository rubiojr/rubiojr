---
title: "lists.sh hacks: automated ~/Lists publishing"
date: 2022-05-18T08:23:33+01:00
draft: false
tags:
  - systemd 
  - linux
  - hacks
---

A dependency free (using built-in Linux distribution tools, that is) mechanism to automatically publish `*.txt` changes when I edit them.
In a nutshell, this script will use a couple of systemd user units to `scp` `.txt` files when there are changes in `$HOME/Lists`.

Desktop notifications when the content is updated remotely are also enabled (optinally disabled).

An up to date version of the script can be found [here](https://github.com/rubiojr/scripts/blob/master/lists-sh-update.sh).

```bash
#!/bin/bash
#
# Automatically scp files to https://lists.sh
# when $HOME/Lists files are changed.
#
# Setup a lists.sh account first (See https://lists.sh)
#
# Usage:
#
#   * Setup a lists.sh account first (See https://lists.sh)
#   * Update the LISTS_PATH variable if you don't want your lists to live in ~/Lists
#   * Run this script
#
set -e

### CONFIGURATION ###

LISTS_PATH="$HOME/Lists"  # Directory holding .txt files
NOTIFY_SEND=1 # Comment out this variable if you don't want notifications

#####################

SDPATH="$HOME/.config/systemd/user"

mkdir "$SDPATH" "$LISTS_PATH" -p

cat > "$SDPATH/lists-sh.path" <<EOF
[Unit]
Description=lists.sh directory

[Path]
PathChanged=%h/Lists
Unit=lists-sh.service

[Install]
WantedBy=default.target
EOF

cat > "$SDPATH/lists-sh.service" <<EOF
[Unit]
Description=Update lists.sh

[Service]
Type=simple
ExecStart=%h/.config/systemd/user/lists-sh.script
EOF

cat > "$SDPATH/lists-sh.script" <<EOF
#!/bin/sh
scp ~/Lists/*txt lists.sh:
if [ -n "$NOTIFY_SEND" ]; then
notify-send "lists.sh content updated!"
fi
EOF
chmod +x "$SDPATH/lists-sh.script"

systemctl --user daemon-reload
systemctl --user enable lists-sh.path
```
