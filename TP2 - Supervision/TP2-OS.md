# TP2 - OS - Supervision

## ISTIC VM
We created two VMs on the ISTIC network, one for monitoring and one for the host to monitor.
```
Monitoring mahcine  : cadam
IP : 148.60.11.181
user : zprojet
psw : zebulonAdam

Host machine : bdezordo
IP : 148.60.11.221
user : zprojet
psw : zebulonDezordo
```

## Monitoring

Monitoring is on cadam VM : `148.60.11.181`

### Installation

We are on Debian 11 and download Checkmk [here](https://checkmk.com/download?method=cmk&edition=cre&version=2.2.0b3&platform=debian&os=bullseye&type=cmk) : 
`Linux / Raw / 2.2.0b3 / Debian / 11`

Pull in the package using wget :

`wget https://download.checkmk.com/checkmk/2.2.0b3/check-mk-raw-2.2.0b3_0.bullseye_amd64.deb`

Install the package using dpkg :

`sudo apt install ./check-mk-raw-2.2.0b3_0.bullseye_amd64.deb`

Create a Checkmk monitoring site :

`sudo omd create monitoring`
```
Created new site monitoring with version 2.2.0b3.cre.

  The site can be started with omd start monitoring.
  The default web UI is available at http://cadam/monitoring/

  The admin user for the web applications is cmkadmin with password: p9Mxp0Vd
  For command line administration of the site, log in with 'omd su monitoring'.
  After logging in, you can change the password for cmkadmin with 'cmk-passwd cmkadmin'.
```

Start the site : `sudo omd start monitoring`
 
I changed the password for cmkadmin with `cmk-passwd cmkadmin` as :
```
user : cmkadmin
password : zebulonAdam
```
### Install the agent

Install the agent from the offered packages: `Setup > Agents > Linux`

Use wget or curl to download the agent on the vm you want to be monitored. Then apt or def to install it.

Check-mk-agent : `/scripts/super-server/1_xinetd : check-mk-agent`

```
wget http://cadam/monitoring/check_mk/agents/scripts/super-server/1_xinetd/check-mk-agent
sudo apt install ./check-mk-agent_2.2.0b3-1_all.deb
```

Check port 6556 is open :  `sudo ss -lnpt` (it was open)

It is not open si start and enable :
```
sudo systemctl enable check_mk.socket
sudo systemctl start check_mk.socket
```

### Add the Host

On the check-mk server, go in `Setup > Hosts > "Add Host" `
Define the hostname (bdezordo.istic.univ-rennes1.fr) and `Save and go to service configuration`



