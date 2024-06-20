# Open Razer and Signed Modules

Loosely adopted from `/usr/share/doc/dkms/README.md`

```
sudo dnf install dkms
sudo mokutil --import /var/lib/dkms/mok.pub
```

It will ask for a password. Pick something quick and easy, it's one time use.

Reboot. A screen will come up for key management. Select Enroll MOK 
-> Continue -> Yes -> Enter the password from `mokutil --import` -> Reboot.

Upon logging back in, run the following command to validate enrollment is
completed:

```
sudo mokutil --test-key /var/lib/dkms/mok.pub
```

Now install openrazer per 
[the online instructions](https://openrazer.github.io/#fedora).

```
sudo dnf config-manager --add-repo https://download.opensuse.org/repositories/hardware:/razer/Fedora_$(rpm -E %fedora)/hardware:razer.repo

sudo dnf install openrazer-meta polychromatic
```

And that should do it.