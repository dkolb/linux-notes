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
