# Unlock LUKS using TPM2

Sources:
  * [0pointer.net Blog Post](https://0pointer.net/blog/unlocking-luks2-volumes-with-tpm2-fido2-pkcs11-security-hardware-on-systemd-248.html)
  * [Understand PCR Banks (Microsoft)](https://learn.microsoft.com/en-us/windows/security/hardware-security/tpm/switch-pcr-banks-on-tpm-2-0-devices)
  * Personal experience with dracut

## Enable fido2 and tpm2-tss in the initramfs

```conf
# /etc/dracut.conf.d/tpm2.conf
add_dracut_modules+=" tpm2-tss "
```

```conf
# /etc/dracut.conf.d/fido2.conf
add_dracut_modules+=" fido2 "
```

Edit `/etc/crypttab`:

If you setup with a whole disk there's just one line. Regardless, you need
to modify the line for your filesystem(s) as follows:

```
luks-7191cf54-576f-4065-b45f-eccdf9354402 UUID=7191cf54-576f-4065-b45f-eccdf9354402 none discard,fido2-device=auto,tpm2-device=auto
```

Note the final column. Take the UUID and paste it into the following commands
to enrol your disk:

```
sudo systemd-cryptenroll \
  --wipe-slot=tpm2 \
  --tpm2-device=auto \
  --tpm2-pcrs='secure-boot-policy:sha384+kernel-boot:sha384+shim-policy:sh384' \
  /dev/disk/by-uuid/7191cf54-576f-4065-b45f-eccdf9354402

sudo systemd-cryptenroll \
  --wipe-slot=fido2 \
  --fido2-device=auto \
  /dev/disk/by-uuid/7191cf54-576f-4065-b45f-eccdf9354402

sudo systemd-cryptenroll \
  --recovery-key \
  /dev/disk/by-uuid/7191cf54-576f-4065-b45f-eccdf9354402
```

It is recommended to set up an alias for both of these as you may or may not
need to redo these enrollments. TPM2 especially will need to be re-enrolled
when you make some changes to your system either in the UEFI, kernel, etc.

Finally, run the following to update the initramfs:

```shell
sudo dracut --force --verbose --regenerate-all
```

Verifiy that `fido2` and `tpm2-tss` are in the list of modules included.

Reboot to test. You can run `sudo systemd-cryptenroll --wipe-slot=tpm2` to 
clear the TPM2 slot to test your FIDO2 key and recovery key.

## Using fido2

Plymouth is kinda trash and won't supply any feedback. You'll only see the
"password prompt". You would enter your FIDO2 key's PIN and then touch it
and...wait to see if anything loads.

(MAYBE)

This can be alleviated by running the following commands:

```
sudo plymouth-set-default-theme details
dracut --force --verbose --regenerate-all
```
## Using recovery key

Don't have any FIDO2 keys plugged into the machine and then enter a bit of
trash into the FIDO2 prompt. You will then be prompted for a passphrase
or recovery key. Enter your recovery key *with the dashes*.