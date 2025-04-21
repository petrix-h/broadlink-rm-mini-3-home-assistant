# broadlink-rm-mini-3-home-assistant
Memo on How to connect Broadlink RM Mini 3 to Home Assistant

Tested with Python 3.11.2 on Debian

Tested with Python 3.9.6 on OS X


## Clone and run python API

```
git clone https://github.com/mjg59/python-broadlink$0
```

```
cd python-broadlink 
```

Make a virtual python environment to keep things separate from host installation

```
python3 -m venv venv 
```

Activate the vitual environment

```
source venv/bin/activate
```

Install the python tooling thing in the venv 

```
pip3 install broadlink 
```

Run python inside the venv

```
python3
```

Get ready to use the API: 

```
import broadlink
```

Eventually, to exit python and deactivate venv:

in Python
```
exit()
```

in venv:
```
deactivate
```

## Connect the RM mini 3

Start by setting up the RM mini 3 in provisioning mode by: 

1. Press and hold the reset button untli the front LED starts flashing rapidly (about 7-10 sec)... 
2. Release the button
3. Press the reset button until the blue led starts flashing less rapidly than before (about 3-ish sec)... 
4. Look for a open wifi network with the name: `BroadlinkProv` and connect to it
5. After making sure you are still connected to the BroadlinkProv wifi in the python terminal give the RM mini 3 the command to connect to a specific wifi, with the password, and 3 means WPA2

```
broadlink.setup('myssid', 'mynetworkpass', 3)
```

The terminal will not give any confirmations... Wait for a bit (not sure if necessary), but the blue light on the RM Mini will eventually stop flashing and it shoudld throw you off the `Prov` network... Now power cycle the RM Mini 3 and check your router if it has given a DHCP lease to something starting with `RMMINI`... note its IP address... 

## Connect to Home Assistant 

With the IP address in hand go to HA Settings -> Devices and Services -> Add Integeation -> Broadlink 

And supply the IP for the RM Mini 3... 

Then go and watch this video, note that in 2025 `Servicess` are replaced by `Actions` in Home assistant, but otherwise a good first how to on how to create and send commands through broadlink devices: https://youtu.be/SjHrHKOUK70?t=346

## Cheat sheet: 

### Learn
- Dev tools -> Actions -> Learn 
- Choose Entity -> RM Mini 3
- Device -> Select -> Name = The device to control, ex. "Samsung TV"
- Command -> Select -> Name = the command to learn, ex "Power"

Click on "Perform Action" -> RM Mini light should be White -> Press the command on remote -> light off = learned

### Run
- Dev tools -> Actions -> Send Command 
- Choose Entity -> RM Mini 3
- Device -> Select -> Name = The device to control, ex. "Samsung TV"
- Command -> Select -> Name = the command to learn, ex "Power"

Click on "Perform Action" -> RM Mini light flashes hopefully recaver also reacts...


# TODO
- Figure out how to manage commands... They are stored under /config/.storage with the name of the IR device_mac_addr... and strings in there.. 
    - this didn't seem to work: https://github.com/xtraorange/hass-broadlink-manager/tree/main$0
    - This could be a alternative: https://github.com/t0mer/broadlinkmanager-docker$0 
