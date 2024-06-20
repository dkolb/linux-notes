# FFXIV Install

While the Flatpak XIVLauncher distribution is probably the best supported
various Dalamud plugins that like to maybe talk to `/usr/bin/say` will
necessitate installing XIVLauncher.Core into the OS itself.

While we're at it, we might as well use a better patched WINE too.

rankynbass's [RBLocalInstaller](https://github.com/rankynbass/RBLocalInstaller)
and [fork of XIVLauncer.Core](https://github.com/rankynbass/XIVLauncher.Core)
will provide something of a smoother experience.

```
gh repo clone rankynbass/RBLocalInstaller
cd RBLocalInstalelr
mkdir $HOME/.local/share/applications
./rbli -l
```

Utilize `espeak` for `/usr/bin/say` so IINACT can do TTS:

```
sudo dnf install espeak
```

Create a script `/usr/bin/say` with the following contents:

```shell
#!/bin/sh

espeak -ven "$1"
```

## Make LMeter/IINACT Do The Cactbot

1. Add the following Dalamud Repositories:
  * `https://gitlab.com/joshua.software.dev/LMeter/-/raw/master/repo.json`
  * `https://raw.githubusercontent.com/marzent/IINACT/main/repo.json`
1. Install IINACT and LMeter Plugins
1. Open Cactbot Configuration in Firefox, configure with TTS.
1. Open Cactbot Raidboss UI in Firefox, test at Summorford Farms w/ `/cd 5`.
   You want to make sure your TTS is working.
1. In Dalamud Settings, 
1. Go to the [TotallyNotCef releases](https://gitlab.com/joshua.software.dev/TotallyNotCef/-/releases)
   and download the most recent "Linux Selfcontained" zip. Extract to
   `$HOME/.xlcore/TotallyNotCef`
1. In `$HOME/.local/bin` (or a similar place on your path and in your home 
   directory), create an executable script named `cactbot-to-lmeter` that runs
   the command 
   ```shell
   #!/bin/sh

   nohup $HOME/.xlcore/TotallyNotCef/TotallyNotCef \
     'https://quisquous.github.io/cactbot/ui/raidboss/raidboss.html?OVERLAY_WS=ws://127.0.0.1:10501/ws' \
     8080 1 1 > $HOME/.xlcore/logs/totallynotcef.log
   ```
1. In `$HOME/.local/share/xivlauncher-core/xivlauncher.sh` add the following:
   ```shell
   #Add this line right before the last line below
   $HOME/.local/bin/cactbot-to-lmeter
   ```