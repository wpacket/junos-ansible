# Junos Config
A collection of scripts to save and push your configuration on Juniper Networks equipements. 

## Explanation & Motivation
I was looking for an easy way to save a restore lab configuration in my Juniper DC lab. 
Considering I was dealing with 10 devices or so,  it was a nightmare to save each config in a local file every time I was dealing with differents topology.
Now I can do that with one CLI in my local shell.

## How does it work
```./junos_config_pull.yml``` : Save the configuration from multiple equipements in a local directory.
```./junos_config_pull.yml``` : Push & commit multiple configuration file from a local directory.

## Before your get started

First of all, complete the host file with your devices IPs and hostname. The hostname value is used for the junos config file  you will eventually push/pull. 
```
[ALL]
10.10.46.70     hostname=qfx10k-sp
10.10.46.71     hostname=qfx10k-spine1      
10.10.46.72     hostname=qfx10k-spine2
10.10.46.74     hostname=qfx10k-leaf2
10.10.46.75     hostname=qfx10k-leaf3
```

Next edit the vars.yml file and complete the username & password to be used on each device to save and restore your configuration
```
configs_dir: ./configs/
ansible_ssh_pass: <PASSWORD>
ansible_ssh_user: <USERNAME>
```

## How to Run it
The playbook can then be run from a local shell. The example below demonstrate the usage of the config_pull.
Once executed, the equipements configuration files will be visible in the ./configs directory.

```
> ansible-playbook junos_config_pull.yml -i hosts
PLAY [Pull Configuration from Junos equipment] *******************************************************************

TASK [include_vars] **********************************************************************************************
ok: [10.10.46.70]
ok: [10.10.46.71]
ok: [10.10.46.72]
ok: [10.10.46.74]
ok: [10.10.46.75]

TASK [Checking devices connectivity] *****************************************************************************
ok: [10.10.46.74]
ok: [10.10.46.70]
ok: [10.10.46.75]
ok: [10.10.46.71]
ok: [10.10.46.72]

TASK [Backup running configuration on each devices and save in  ./configs/ directory] ****************************
changed: [10.10.46.74]
changed: [10.10.46.72]
changed: [10.10.46.71]
changed: [10.10.46.70]
changed: [10.10.46.75]

PLAY RECAP *******************************************************************************************************
10.10.46.70                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
10.10.46.71                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
10.10.46.72                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
10.10.46.74                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
10.10.46.75                : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```