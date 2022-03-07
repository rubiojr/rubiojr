---
title: "PinePhone Pro with Mobian as an AirPlay speaker"
date: 2022-03-06T16:37:04+01:00
draft: false
tags:
  - mobian
  - pinephone
---

Tested on **shairport-sync 3.3.8**, currently available on mobian/bookworm.

The process is pretty similar to what you'd do on a Raspberry Pi.

### Install and configure shairport-sync

```sh
apt install shairport-sync
```

Edit `/etc/shairport-sync.conf` to set the backend to `pulseaudio` with:

```ini
output_backend = "pa";
```

### Create a systemd user service and start it:

```sh
mkdir ~/.config/systemd/user
```

Edit `~/.config/systemd/user/shairport-sync.service` and add:

```ini
[Unit]
Description=Shairport Sync - AirPlay Audio Receiver

[Service]
ExecStart=/usr/bin/shairport-sync
Restart=on-failure

[Install]
WantedBy=default.target
```

Enable and start the new service:


```sh
systemctl --user enable --now shairport-sync
```

### User lingering (optional but recommended)

Enable default user (`mobian`) lingering, so the speaker runs even when you haven't logged in:

```
sudo loginctl enable-linger mobian
```

## Related

* https://www.pine64.org/pinephonepro/
* https://github.com/mikebrady/shairport-sync
* https://mobian-project.org
* https://blog.mobian-project.org/posts/2021/12/28/pinephone-pro/
