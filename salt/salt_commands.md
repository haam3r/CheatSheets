# Useful Salt command line snippets

## salt-key

`salt-key --list-all` - List keys

`salt-key -r salt.haamer.ae` - Remove keys

----------

## salt-run

`salt-run manage.up` - View alive minions

`salt-run manage.status` - View status of all minions

----------

## salt

`salt '*' grains.items` - View grains data for minions

`salt '*' grains.ls` - List grain types

`salt '*' cmd.run "hostname -f; bash /etc/update-motd.d/90-updates-available; bash /etc/update-motd.d/98-reboot-required; df -h / /boot | tail -n 2 | awk '{print \$6 \" \" \$5}'"` - List hostname, updates, wether reboot is required and free disk space for all nodes

----------

## salt-call

Use salt to set up the cron job to run salt between 2 and 2:59 am each day:

```salt
/usr/bin/salt-call -l quiet state.highstate:
  cron.present:
    - user: root
    - minute: random
    - hour: 2
```
