# Debian Unattended Upgrades

```
sudo apt update
sudo apt install unattended-upgrades apt-listchanges mailutils ssmtp
```
In the file `/etc/apt/apt.conf.d/50unattended-upgrades` in the section
`Unattended-Upgrade::Origins-Pattern {` uncomment the line
`origin=Debia,codename=${distro_codename}-updates";`.

Uncomment `Unattended-Upgrade::Mail "":` and put your email address in place
of the empty string. Beneath it add the following:

```
// Specify the from email address.
Unattended-Upgrade::Sender "Unattended Upgrades <fromAddress@domain.com>";
```
In the file `/etc/ssmtp/ssmtp.conf`:

Change the line `root=postmaster` to `root=your@email.com`

Beneath `mailhub=mail` add the following:

```
mailhub=smtp.example.com:465
AuthUser=username
AuthPass=password
UseTLS=YES
UseSTARTTLS=NO
```

Uncomment `rewriteDomain=` and set it to the domain of your email.

Uncomment `FromLineOverride=YES`.

Run the following command to test:

```
echo 'Test email' | mailx -s 'Test email' 'your@email.com'
sudo unattended-upgrade -d
```