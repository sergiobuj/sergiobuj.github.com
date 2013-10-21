---
layout: post
title: "Hubzero Submit Installation"
date: 2011-11-01 16:45
comments: true
categories:
- batch system
- condor
- instalacion
- python
- submit

---




There are two machines/locations involved in the installation. The hub as **HUB** and the cluster controller as **CLUSTER**.
Through this installation the **$HOME** directory on **CLUSTER** will refer to the home directory of the user that will be used to submit the jobs to the cluster (for most purposes would be any user other than root).

INSTALLATION
=====

HUB
---

```
apt-get install hubzero-app-submit
apt-get install hubzero-submit-server
apt-get install hubzero-submit-distributor
```

---------

MOVING FILES AROUND
===
Files can be easily transfered between *HUB* and *CLUSTER* using the `scp` command.

HUB
---

`cd` to the submit directory (`/opt/submit` by default) and add the public key found in `ssh/submit_rsa.pem` to the authorized keys file on *CLUSTER* _$HOME_`/.ssh/authorized_keys`.

CLUSTER:
---
Create directories `submit`, `log`, `bin` and `scratch`.

```
$ mkdir -p $HOME/submit/log
$ mkdir -p $HOME/bin
$ mkdir -p $HOME/scratch
```

HUB:
---
Copy the scripts needed for the Batch System that is going to be used from `BatchMonitors/<YOUR BATCH SYSTEM>` directory to *CLUSTER* _$HOME_`/submit`

Copy file `LogMessage.py` to *CLUSTER* _$HOME_`/submit`

Copy files from the `Scripts/<YOUR BATCH SYSTEM>` directory to *CLUSTER* _$HOME_`/bin`

In case it exists, remove file `/etc/submit/config`

CLUSTER:
---
Make sure the files in _$HOME_`/bin` are executable, this can simply be done using:
`$ chmod +x $HOME/bin/*`

HUB:
---
Check the ownership of the file `monitorJobDB`, it should belong to `gridman` and group `gridman`.
To change it simply `# chown gridman.gridman monitorJobDB`

Use `visudo` to give permissions over the `bin/update_known_hosts` file adding this entry:
`ALL ALL=NOPASSWD:/opt/submit/bin/update-known-hosts`

* The path should be changed in case that the installation was made on a different directory.

---------

FILL IN CONFIGURATION FILES
===

HUB:
---
Add information to `sites.dat`

```
[venue_name]
venues = <comma separated list of venues>
remoteBatchSystem = <PBS or CONDOR or...>
remotePpn = <Has to be specified, if unknown set to 1>
checkProbeResult = false
remoteScratchDirectory = <path on CLUSTER $HOME/scratch>
siteMonitorDesignator = monitor_name
venueMechanism = <ssh or local>
remoteUser = <username on the CLUSTER>
```

Add information to `monitors.dat`

```
[monitor_name]
venue = <venue_name>
remoteUser = <username on the CLUSTER>
venueMechanism = <ssh or local>
remoteMonitorCommand = <path on CLUSTER $HOME/submit/MONITOR-SCRIPT>
```

* Where `MONITOR-SCRIPT` should be changed to match the script according to your Batch System.

---------

EDIT FILES
===

HUB:
---
Edit file `/etc/hubzero/maxwell.conf` and add the lines following lines (3) before the hub information.

```
visualization_params="""
submit_target tcp://<SUBMIT_SERVER_LOCATION>:830
"""
```

CLUSTER:
---
Edit the monitor file on _$HOME_`/submit/MONITOR-SCRIPT` to set this variables.

```
SITEDESIGNATOR = monitor_name #Same as HUB monitors.dat
MONITORROOT = <Adjust path to $HOME/submit>
MONITORLOGLOCATION = <Adjust path to $HOME/submit/log>
```

If you are using CONDOR and the executables are already in the user PATH set this:

```
CONDOR_ROOT = ""
CONDOR_CONFIG = ""
```


---------

DONE!
===
*HUB:* Start or Restart `submon` and `submit-server` daemons.

```
# /etc/init.d/submit-server start
# /etc/init.d/submon start
```
