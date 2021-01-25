# PinePhone running Manjaro Linux

## backlight off

echo 1 > /sys/class/backlight/backlight/bl_power

## Saving memory/resources

Without disabling services or uninstalling packages.

Note this will kill your graphical environment.

```
sudo systemctl stop phosh
pkill -f goa-daemon
pkill -f goa-identity-service
pkill -f evolution-source-registry
pkill -f evolution-data-server
pkill -f gvfs-udisks2-volume-monitor
pkill -f feedbackd
pkill -f gvfsd-fuse
```

## shairport-sync

Using the pinephone as an AirPlay receiver/speaker.

```
systemcl enable avahi-daemon
```

Relevant `/etc/shairport-sync.conf` config, required to avoid choppy audio or airplay breaking.

```
general =
{
  name = "pinephone";
  interpolation = "basic";
  output_backend = "alsa";
  mdns_backend = "avahi";
};

alsa =
{
  output_device = "hw:PinePhone";
};
```
