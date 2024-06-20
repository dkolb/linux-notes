# Swapping SDDM to GreetD

Sometimes SDDM and amdgpu get mad as is the case in kernel's 6.9.4 and 6.9.5.
Swapping to greetd and gtkgreet can help.

```
sudo dnf install greetd gtkgreet
sudo systemctl disable sddm
sudo systemctl enable greetd
```

Drop in config files in `/fedora-40/swap-to-greetd-resources`