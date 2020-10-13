# HBlink

HBlink is software for control of traffic generated by DMR radios.

It allowed communication from DMR radio via Pi-Star via HBlink to another Pi-Star and DMR radio connected to it.
It allowed communication from DMR radio via Pi-Star via HBlink to BrandMeister server as peer and DMR radios connected to talk groups in BrandMeister server.
It allowed communication from DMR radio via Pi-Star via HBlink to IPSC2 server as peer and DMR radios connected to talk groups in IPSC2 server.
It allowed communication from DMR radio via Pi-Star via HBlink to XLX as peer and DMR or D-Star radios connected to XLX.
It allowed communication from DMR radio via Pi-Star via HBlink to HBlink as peer and DMR radios connected to talk groups in HBlink.

Originaly this software was created as software for connection between IPSC2 and BrandMeister.

In our days this is unnecesarry. There is no problem if some network want, to negotiate with another and build such connection.

For what is applicable to use this software?

THis is light weight software. It work without any problem on Raspbeppy Pi 3. It is so light, so you can install more software, like Apache, and build your own website, Smokeping for monitoring connection of your device to Internet, possible to add DMR host and turn your Raspberry into repeater and control the traffic via HBlink... 
Variants of ussage of this software are so many...
Unfortunately when someone make something good, come a lot of bad peoples, who start complain, use the software inapropriate and at the end this lead to close such nice projects.
In IPSC2 networks is spread a email that make HBlink illegal. In this networks, if admin ocure HBlink connected to his server, this HBlink is banned. Not all but many of BrandMeister admins also do the same.
I complete do not understand such war aganist radio hams who want to learn how servers work.
My opinion about this is - Knowleage about servers and how they work, knowleage about this how ourdays digital communication work can not belong only to small group of choisen gurus. This knowleage must belong to all radio hams. All radio hams have right to learn and increase their experience in communications, special in digital communication.
The main target of radio hams is education and self education in communication.
Creator of HBlink N4IRS by unknow for me reason, removed repository for HBlink.
I do not mind that it is good idea.
Pressure over peoples who make such software and peoples who use such software is unbelievable. 
So here i publish my copy of this software.
Because im not creator this software is published without any support. You can use it on your own risk.

Follow next command in terminal to install HBlin and HBmonitor on raspbian buster or Linux Mint:

apt update

apt upgrade

apt dist-upgrade

apt autoremove

apt autoclean

#install hblink

apt install git

apt install python3-distutils

cd /opt/

wget https://bootstrap.pypa.io/get-pip.py

python3 get-pip.py

apt install python3-twisted

apt install python3-bitarray

apt install python3-dev

git clone https://github.com/lz5pn/HBlink

cd /opt/HBlink

mv dmr_utils3 /opt/

mv HBlink3 /opt/

mv HBmonitor /opt/

cd /opt/

rm -r HBlink

cd /opt/dmr_utils3

chmod +x install.sh

./install.sh

cd /opt/HBlink3

cp hblink-SAMPLE.cfg hblink.cfg

cp rules-SAMPLE.py rules.py

Autostart HBLink:

nano /lib/systemd/system/hblink.service

Copy and paste the next:

------------------------------------------------------------------------------------------------------------------------
[Unit]

Description=Start HBlink

After=multi-user.target

[Service]

ExecStart=/usr/bin/python3 /opt/HBlink3/bridge.py

[Install]

WantedBy=multi-user.target

------------------------------------------------------------------------------------------------------------------------

systemctl daemon-reload

systemctl enable hblink


Install Parrot for Echotest:

chmod +x playback.py


Create directory for registration files, if /var/log/hblink is not created.

mkdir /var/log/hblink

To start Parrot service must use file /lib/systemd/system/parrot.service 

nano /lib/systemd/system/parrot.service

Copy and paste the next:

------------------------------------------------------------------------------------------------------------------------
[Unit]

Description=HB bridge all Service

After=network-online.target syslog.target

Wants=network-online.target

[Service]

StandardOutput=null

WorkingDirectory=/opt/HBlink3

RestartSec=3

ExecStart=/usr/bin/python3 /opt/HBlink3/playback.py -c /opt/HBlink3/playback.cfg

Restart=on-abort

[Install]

WantedBy=multi-user.target

------------------------------------------------------------------------------------------------------------------------

Start Parrot service:

systemctl enable parrot.service

systemctl start parrot.service

systemctl status parrot.service

nano /opt/HBlink3/rules.py

Test configuration:

python3 /opt/HBlink3/bridge.py

systemctl start hblink

systemctl status hblink

Install web monitor for HBLink.

cd /opt/HBmonitor

chmod +x install.sh

./install.sh

cp config_SAMPLE.py config.py

nano /opt/HBmonitor/config.py

Start monitor as system service:

cp utils/hbmon.service /lib/systemd/system/

systemctl enable hbmon

systemctl start hbmon

systemctl status hbmon

forward TCP ports 8080 and 9000 in router firewall


My HBlink http://kario88.dynamic-dns.net:8184/


73 de LZ5PN
