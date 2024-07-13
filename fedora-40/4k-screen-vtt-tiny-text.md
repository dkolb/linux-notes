# My 4K Monitor Means I Can't Read VTT Text!

I get it! I too have The Old(TM)!

Dig into `/etc/default/grub` and drop an argument on the GRUB_CMD_LINE_LINUX
variable. Plunk down a `video=1920x1080` (or `2560x1440`) at the end of it.

Run the following:

```shell
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

## Immutable Fedora Variants
```shell
sudo rpm-ostree kargs --editor
```