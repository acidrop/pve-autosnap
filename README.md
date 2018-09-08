# pve-autosnap
 Proxmox automatic snapshot tool 

## Usage

```bash
Usage:    pve-snap --mode [--storage STORAGENAME] [--leave NUMBER] [--sleep=NUMBER] [--exclude=VMS]

Example:  pve-snap --weekly --storage=ceph --leave=2 --sleep=10
     or:  pve-snap --yearly --storage=pve-data
     or:  pve-snap -d -s stor -l 4

Arguments:
    -d, --daily                 Run this script in mode for daily autosnapshots
    -w, --weekly                Run this script in mode for weekly autosnapshots
    -m, --monthly               Run this script in mode for monthly autosnapshots
    -y, --yearly                Run this script in mode for yearly autosnapshots
    -s, --storage=STORAGENAME   Specify the storage name for which will be used auto snapshots
                                (not specified will enable for all storages)
    -l, --leave=NUMBER          Specify the number of snapshots which should will leave, anything longer will be removed
                                (0 or not specified will disable removing snapshots)
    -D, --sleep=NUMBER          Specify the modifier for sleep that would create a delay after each snapshot operation
                                (calculate as: NUMBER gigabytes per minute, min 1 minute)
    -E, --exclude=VMS           Specify the comma separated list of VMS for exclude
```

## Enable pve-autosnap

Just add your rules to crontab, example:
```bash
STORAGE=local

crontab -l > crontab.txt

cat >> crontab.txt << EOF
# Daily snapshot
0 3 * * 1-6 /bin/pve-autosnap --daily --storage=$STORAGE --leave=3
# Weekly snapshot
0 3 * * 7 /bin/pve-autosnap --weekly --storage=$STORAGE --leave=3
# Monthly snapshot
0 3 1 * * /bin/pve-autosnap --monthly --storage=$STORAGE --leave=4
EOF

crontab crontab.txt
rm crontab.txt
```
