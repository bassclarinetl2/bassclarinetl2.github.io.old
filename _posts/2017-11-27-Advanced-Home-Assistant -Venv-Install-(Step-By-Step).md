---

layout: post
title: Advanced Home Assistant Venv Install (Step By Step)
date: 2017-11-27

---

Since I tend to install my hass enviroment using the Venv method I figured I'd leave myself notes on how to do so and share them.  Steps are derived from [Here](https://home-assistant.io/docs/installation/virtualenv/) and I have omitted the $ from the code blocks listed below, commands requiring root will have sudo.


## {% linkable_title Step 1: Install dependencies %}

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3-pip python3-dev
sudo pip3 install --upgrade virtualenv
```

## {% linkable_title Separate user & group for Home Assistant %}

Standard linux practice is to give services their own user/group to limit permissions.

```bash
sudo adduser --system homeassistant
sudo addgroup homeassistant
```

## {Add Zwave Permissions}

```bash
sudo usermod -G dialout -a homeassistant
```

## {% linkable_title Create installation directory for Home Assistant %}

```bash
sudo mkdir /srv/homeassistant
sudo chown homeassistant:homeassistant /srv/homeassistant
sudo su -s /bin/bash homeassistant

python3 -m venv /srv/homeassistant
```

## {% linkable_title Install or update Home Assistant %}


```bash
source /srv/homeassistant/bin/activate
```

Within the homeassistant venv:
```bash
pip3 install --upgrade homeassistant
```

Exit venv
```bash
exit
```


## {Configure Autostart}
```bash
sudo nano /etc/systemd/system/home-assistant.service
```

Paste in
```	
[Unit]
Description=Home Assistant
After=network-online.target

[Service]
Type=simple
User=%i
ExecStart=/srv/homeassistant/bin/hass -c "/home/homeassistant/.homeassistant"

[Install]
WantedBy=multi-user.target
```

Press Ctrl + X and save file.

```bash
sudo systemctl --system daemon-reload
sudo systemctl enable home-assistant
```

Start per usual with systemctl