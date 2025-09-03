enable
configure terminal

interface GigabitEthernet0/0
 nameif outside
 security-level 0
 ip address 203.0.113.2 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 nameif inside
 security-level 100
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/2
 nameif dmz
 security-level 50
 ip address 192.168.2.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/3
 nameif server
 security-level 75
 ip address 192.168.3.1 255.255.255.0
 no shutdown

route outside 0.0.0.0 0.0.0.0 203.0.113.1

object network INSIDE-NET
 subnet 192.168.1.0 255.255.255.0
 nat (inside,outside) dynamic interface

object network DMZ-SERVER
 host 192.168.2.10
 nat (dmz,outside) static 203.0.113.100

access-list OUTSIDE-IN permit tcp any host 192.168.2.10 eq 80
access-list OUTSIDE-IN permit tcp any host 192.168.2.10 eq 443
access-group OUTSIDE-IN in interface outside

access-list INSIDE-TO-SERVER permit ip 192.168.1.0 255.255.255.0 192.168.3.0 255.255.255.0
access-group INSIDE-TO-SERVER in interface inside

write memory
