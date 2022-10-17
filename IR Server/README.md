## Use the following instruction on your server</br></br>
#### 1. update & upgrade the packages </br>
```shell script
sudo apt update && apt upgrade -y
``` 
#### 2. install iptables persistent </br>
```shell script
sudo apt install iptables-persistent
``` 
#### 3. edit this file: </br>
```shell script
sudo nano /etc/iptables/rules.v4
``` 
#### 4. Add the following to the `rules.v4` file and save:</br>
```shell script
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A FORWARD -d xx.xx.xx.xx -p tcp -m tcp --dport 80 -j ACCEPT
-A FORWARD -d xx.xx.xx.xx -p udp -m udp --dport 80 -j ACCEPT
COMMIT
*nat
:PREROUTING ACCEPT [0:0]
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
-A PREROUTING -p tcp -m tcp --dport 80 -j DNAT --to-destination xx.xx.xx.xx
-A PREROUTING -p udp -m udp --dport 80 -j DNAT --to-destination xx.xx.xx.xx
-A POSTROUTING -d xx.xx.xx.xx -p tcp -m tcp --dport 80 -j MASQUERADE
-A POSTROUTING -d xx.xx.xx.xx -p udp -m udp --dport 80 -j MASQUERADE
COMMIT
``` 
#### 5. Replace `NON_IR_IP` with your non-IR server IPv4 and run the command:</br>
```shell script
sudo sed -i 's/xx.xx.xx.xx/NON_IR_IP/g' /etc/iptables/rules.v4
```
This will replace all occurences of `xx.xx.xx.xx` with the actual IP address of your non-IR server.</br>
#### 6. Open this file:</br>
```shell script
sudo nano /etc/sysctl.conf
```
Add the following line to the beginning of the file (or just find it and uncomment it) and save:</br>
```shell script
net.ipv4.ip_forward=1
```
#### 7. Run next two commands to make the changes take effect right away:</br>
```shell script
sudo sysctl -p
```
```shell script
sudo systemctl restart iptables.service
```
