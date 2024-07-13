# Disable the touchpad acting as mouse

## Arch Wiki approach
As with many esoteric "please stop doing this" requests, you'll reference
this section of the Arch wiki:

[Gamepads: Playstation 4 contoller, Disable touchpad acting as mouse](https://wiki.archlinux.org/title/Gamepad#Disable_touchpad_acting_as_mouse)

```conf
# Disable DS4 Touchpad acting as mouse
## Bluetooth
ATTRS{name}=="DualSense Wireless Controller Touchpad", ENV{LIBINPUT_IGNORE_DEVICE}="1"
```

This doesn't seem to fix Steam Input though.

## ds-inhibit

```shell
git clone https://gitlab.com/evlaV/ds-inhibit.git
sudo make install
```