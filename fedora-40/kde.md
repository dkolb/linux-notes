# KDE Stuff
## File Sharing

```
# Run the following
sudo dnf install kdenetwork-filesharing
sudo gpasswd -a <username> usershares
sudo systemctl enable smb
sudo systemctl start smb
```

Log out and log in. Go into the firewall and allow the samba service through.

I never got any of this to work just use Syncthingy.

## Silverblue/Universal Blue/Bazzite Customizations

To customize SDDM either install themes to `/etc/sddm/themes` or copy the 
default themes from `/usr/share/sddm/themes` to `/etc/sddm/themes` and update
the `theme.conf` to point to a different background png.

Then edit `/etc/sddm.conf.d/kde_settings.conf` and set the `[Theme]` settings
as follows:

```ini
[Theme]
ThemeDir=/etc/sddm/themes
Current=01-breeze-fedora
```
Do not change SDDM settings in the KDE System Settings or you will have to
change these settings back manually.