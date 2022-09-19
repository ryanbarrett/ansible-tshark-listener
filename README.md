# ansible-tshark-listener

The goal is to provide an hourly/daily/weekly packet trace to provide pcap files for analysis.

**To get started:**
Set up a machine with 2 interfaces (VM, laptop, or raspberry pi).

Specify one interface to listen on and attach this interface to a TAP or SPAN/mirrored port. (i.e. listener_int )


## Requirements

```
ansible-galaxy collection install geerlingguy.ntp
```

## Variables
vars/main.yml
```
server_name: tshark

# Set your timezone if desired
#ntp_timezone: Etc/UTC
ntp_timezone: America/Chicago

# Setup user
ansible_user_name: Tarmac3938

# This user will be set up to capture packets
## view status with tmux:
## sudo su - tshark_user
## tmux attach-session
user_name: tshark_user

# aux interface to set in promiscious mode
listener_int: ens19


### tshark settings ###
# seconds 86400 = 1 day 3600 = 1 hour
tshark_interval: 86400
# quantity of rotating files
tshark_files: 7
# directory location of pcap files
tshark_buffer_dir: /tshark

tshark_options: "-i {{ listener_int }} -b interval:{{ tshark_interval }} -b files:{{ tshark_files }} -w {{ tshark_buffer_dir }}/trace.pcap"

```
# Run the playbook

``` ansible-playbook -i hosts.yml playbook.yml```

## Proxmox Settings
*Ignore this section if you are not using proxmox.*

Some additional settings are needed in proxmox.

```
ip link set $IFACE promisc on

brctl setageing vmbr3 0
brctl setfd vmbr3 0

```

### Persistance settings



 https://forum.proxmox.com/threads/network-sensor-monitor-session-promiscuous.37845/
 ```
/etc/network/interfaces
...
iface enxe inet manual
        up /sbin/ip link set $IFACE promisc on
#NIC_3_SPAN
...
auto vmbr3
iface vmbr3 inet manual
        bridge-ports enxe
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094
        up /usr/sbin/brctl setageing vmbr3 0
        up /usr/sbin/brctl setfd vmbr3 0
#SPAN bridge
 ```

# Disclaimer

I hacked this together, for my needs, in a couple of hours. It's not meant for production use.

This has only been tested for my sandbox environment. Your results may vary.
