# Mycroft WiFi Setup

This repository holds Mycroft's wifi setup client. To build it, run `./build.sh`. To create a deb package, after building it run `./package_deb.sh`. The results are placed into the `dist/` folder.

# modification for Debian stretch
Mycroft Wifi Setup does not run under Debian Stretch because wpa_supplicant is no longer compiled with AP-mode support in the Stretch distro. This fork provides a proof of concept for Wifi setup running under Debian Stretch. Main change is a customized version wpa_supplicant that is compiled with AP-mode support...

How to get it running:
1) Mycroft installation:

```
git-install into ~/mycroft-core
```

2) Mycroft Wifi Setup installation:

```
cd ~
git clone https://github.com/domcross/mycroft-wifi-setup.git
cd ~/mycroft-wifi-setup
./setup.sh
sudo touch /var/lib/misc/dnsmasq.leases
sudo chown -R root:root  /var/lib/misc/
```

3)  initialize `/etc/wpa_supplicant/wpa_supplicant.conf` with following content:
```
ctrl_interface=/var/run/wpa_supplicant
driver_param=p2p_device=1
update_config=1
device_name=PICROFT
device_type=1-0050F204-1
p2p_go_intent=10
p2p_go_ht40=1

network={
        ssid="MYCROFT"
        psk="12345678"
        proto=RSN
        key_mgmt=WPA-PSK
        pairwise=CCMP
        auth_alg=OPEN
        mode=3
        disabled=2
}
```

4) fire up „custom wpa_supplicant“:

```
sudo killall wpa_supplicant
sudo ~/mycroft-wifi-setup/wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf -D nl80211
```

5) run Mycroft with Admin Service and Wifi Setup:

```
cd ~/mycroft-wifi-setup
source ~/mycroft-core/venv-activate.sh
~/mycroft-core/start-mycroft.sh all
./mycroft_admin_service.py &
./run.sh
```

Wifi „MYCROFT“ should be visible, connect to it with password „12345678“, then follow the „captive login process“…

When wifi-setup it returns to the console but admin-service is still running in the background. You can stop it by issuing command `fg 1`, then pressing `Ctrl+C`several times.

Only issue remaining is how to package and deploy ‚wpa_supplicant‘. When this is solved I think the solution will run on any Debian/Stretch system, e.g. Mark-I, Picroft, etc.
