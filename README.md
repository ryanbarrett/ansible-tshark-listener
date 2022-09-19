# ansible-tshark-listener



## Requirements

 ansible-galaxy collection install community.general
 ansible-galaxy collection install geerlingguy.ntp

 ## Variables

 ```
server_names: tshark

# Set your timezone if desired
ntp_timezone: Etc/UTC

# This user will be set up to capture packets
user_name: Tarmac3938

# aux interface to set in promiscious mode
listener_int: ens19

### Tshark settings ###
# seconds 86400 = 1 day 3600 = 1 hour
tshark_interval: 86400
# quantity of rotating files
tshark_files: 7
# directory location of pcap files
tshark_buffer_dir: /tshark

tshark_options: "-i {{ listener_int }} -b interval:{{ tshark_interval }} -b files:{{ tshark_files }} -w {{ tshark_buffer_dir }}/trace.pcap"
 ```


 ## Proxmox Settings

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