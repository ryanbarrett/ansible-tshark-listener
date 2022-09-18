# ansible-tshark-listener



## Requirements

 ansible-galaxy collection install community.general
 ansible-galaxy collection install geerlingguy.ntp

 ## Variables

 ```
# This user will be set up to capture packets
user_name: Tarmac3938

server_names: tshark
# aux interface to set in promiscious mode
listener_int: ens19

# seconds 86400 = 1 day 3600 = 1 hour
tshark_interval: 86400
# quantity of rotating files
tshark_files: 7
# directory location of pcap files
tshark_buffer_dir: /tshark
 ```