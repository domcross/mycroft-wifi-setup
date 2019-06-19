# Mycroft WiFi Setup

This repository holds Mycroft's wifi setup client. To build it, run `./build.sh`. To create a deb package, after building it run `./package_deb.sh`. The results are placed into the `dist/` folder.

# modification for Debian stretch
Mycroft Wifi Setup does not run under Debian Stretch because wpa_supplicant is no longer compiled with AP-mode support in the Stretch distro. This fork provides a proof of concept for Wifi setup running under Debian Stretch. Main change is a customized version wpa_supplicant that is compiled with AP-mode support...

How to get it running:
1) Mycroft installation:
`git-install into ~/mycroft-core`

2) Mycroft Wifi Setup installation:
```cd ~
git clone https://github.com/domcross/mycroft-wifi-setup.git
cd ~/mycroft-wifi-setup
./setup.sh```

3) fire up „custom wpa_supplicant“:
```sudo killall wpa_supplicant
sudo ~/mycroft-wifi-setup/wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf -D nl80211```

4) run Mycroft with Admin Service and Wifi Setup:
```cd ~/mycroft-wifi-setup
source ~/mycroft-core/venv-activate.sh
~/mycroft-core/start-mycroft.sh all
./mycroft_admin_service.py
./run.sh```

Wifi „MYCROFT“ should be visible, connect to it with password „12345678“, then follow the „captive login process“…

Only issue remaining is how to package and deploy ‚wpa_supplicant‘. When this is solved I think the solution will run on any Debian/Stretch system, e.g. Mark-I, Picroft, etc.
