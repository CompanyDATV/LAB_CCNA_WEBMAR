<h1 align="center"&text_color=DC143C>Welcome To DATV</h1>
<center>

![Screenshot_90](https://user-images.githubusercontent.com/83489434/130717671-01b587ff-1904-43e1-811d-a1e305861ba6.png)

</center>

### Below WE will give the network topology of WEBMAR company specializing in creating and maintaining websites:
```diff
- Bảng địa chỉ : Address Tables!
```
<center>

|Device (Name)    |Interfaces|Address IP|subnet mask|Default Gateway|
|:---:|:---:|:---:|:---:|:---:|
| Switch 1        |VLAN 99|172.20.99.11|255.255.255.0|172.20.99.1|
| Switch 2        |VLAN 99|172.20.99.12|255.255.255.0|172.20.99.1|
| Switch 3        |VLAN 99|172.20.99.13|255.255.255.0|172.20.99.1|
| R1              |Fa0/0  |172.20.50.1 |255.255.255.0|ND         |
| R1              |Fa0/1  |No shutdown |             |ND         |
| PC1             |Ncat   |172.20.10.21|255.255.255.0|172.29.10.1|
| PC2             |Ncat   |172.20.20.11|255.255.255.0|172.20.20.1|
| PC3             |Ncat   |172.20.30.11|255.255.255.0|172.20.30.1|
| Server WEB/TFTP |Ncat   |172.20.50.11|255.255.255.0|172.20.50.1|
</center>

```diff
!! Chuyển nhượng cổng - Chuyển đổi: Chuyển đổi 2 - Port assignment - Switch: Switch 2
```

<center>

|Ports      |Assignment                         |Network|
|:---:|:---:|:---:|
| Fa0/1-0/4|802.1q aggregations (native VLAN 99)|172.20.99.0/24|
| Fa0/5-0/10|VLAN 30 - Invite (default)         |172.20.30.0/24|
| Fa0/11-0/17|VLAN 10 - Development-web         |172.20.10.0/24|
| Fa0/18-0/24|VLAN 20 - Designers               |172.20.20.0/24|

</center>

```diff
@ Interface configuration table - Router R1
```

<center>

|Interfaces|Assignment|Address IP|
|:---:|:---:|:---:|
| Fa0/1.1|VLAN 1|172.20.1.1/24|
| Fa0/1.10|VLAN 10|172.20.10.1/24|
| Fa0/1.20|VLAN 20|172.20.20.1/24|
| Fa0/1.30|VLAN 30|172.20.30.1/24|
| Fa0/1.99|VLAN 99|172.20.99.1/24|

</center>

# Solve - Questions

```diff
+ Give the command lines that allow you to delete the current switch configurations.
    - Switch#erase startup-config
    - Switch#reload
- Give the command lines that allow to disable all the ports of the switches
    - Switch(config)#interface range fastEthernet 0/1-24
    - Switch(config-if-range)#shutdown
    - Switch(config)#interface range gigabitEthernet 0/1-2
    - Switch(config-if-range)#shutdown
! Give the command lines that allow you to reactivate the active user ports on Switch2 in access mode.
    - S2(config)#interface range fastEthernet 0/5-24
    - S2(config-if-range)#no shutdown
+ Give the configuration of the switches Switch 2 according to the table address and the following instructions:

• Configure the switch hostname.
• Disable DNS lookup.
• Set pass as the active secret password.
• Configure the webmar password for console connections.
• Configure the webmar password for vty connections.
• Configure the default gateway on each switch
    - // configure the host name:
    - Switch (config) #hostname S2
    -// Disable DNS lookup
    - S2 (config) #no ip domain-lookup
    - // configure the secret password of the privileged EXEC mode
    - S2 (config) #enable secret pass
    - // configure the password for the console line
    - S2 (config) #line console 0
    - S2 (config-line) #password webmar
    - S2 (config-line) #login
    - // configure the password for the vty lines
    - S2 (config) #line vty 0 15
    - S2 (config-line) #password webmar
    - S2 (config-line) #login
    - // configure the default gateways on the three switches
    - S1 (config) #ip default-gateway 172.20.99.1
    - S2 (config) #ip default-gateway 172.20.99.1
    - S3 (config) #ip default-gateway 172.20.99.1
- Configure the VTP protocol on the three switches using the following table

    + //pour Switch1 :

    + S1(config)#vtp mode server
    + S1(config)#vtp domain DomVTP
    + S1(config)#vtp password webmar

    + //pour Switch2

    + S2(config)#vtp mode client
    + S2(config)#vtp domain DomVTP
    + S2(config)#vtp password webmar

    + //pour Switch3
    + S3(config)#vtp mode client
    + S3(config)#vtp domain DomVTP
    + S3(config)#vtp password webmar

+ Configure ports Fa0 / 1 through Fa0 / 5 as trunk ports and designate VLAN 99 as the native VLAN for these trunks.

    + //pour Switch1 :

    + S1(config)#interface range fastEthernet 0/1-5
    + S1(config-if-range)#switchport mode trunk
    + S1(config-if-range)#switchport trunk native vlan 99

    + //pour Switch2 :

    + S2(config)#interface range fastEthernet 0/1-4
    + S2(config-if-range)#switchport mode trunk
    + S2(config-if-range)#switchport trunk native vlan 99

    + //pour Switch3 :

    + S3(config)#interface range fastEthernet 0/1-4
    + S3(config-if-range)#switchport mode trunk
    + S3(config-if-range)#switchport trunk native vlan 99

! Configure the following VLANs on the VTP server:

    - S1(config)#vlan 99
    - S1(config-vlan)#name direction
    - S1(config-vlan)#vlan 10
    - S1(config-vlan)#name developpeur-web
    - S1(config-vlan)#vlan 20
    - S1(config-vlan)#name Designers
    - S1(config-vlan)#vlan 30
    - S1(config-vlan)#name invite

- Configure the management interface address on all three switches.

    + //pour Switch 1 :

    + S1(config)#interface vlan 99
    + S1(config-if)#ip address 172.20.99.11 255.255.255.0
    + S1(config-if)#no shutdown

    + //pour Switch 2

    + S2(config)#interface vlan 99
    + S2(config-if)#ip address 172.20.99.12 255.255.255.0
    + S2(config-if)#no shutdown

    + //pour Switch 3 :

    + S3(config)#interface vlan 99
    + S3(config-if)#ip address 172.20.99.13 255.255.255.0
    + S3(config-if)#no shutdown

+ Assign switch ports to VLANs on Switch 2.

    - S2(config)#interface range fastEthernet 0/5-10
    - S2(config-if-range)#switchport mode access
    - S2(config-if-range)#switchport access vlan 30
    - S2(config-if-range)#exit
    - S2(config)#interface range fastEthernet 0/11-17
    - S2(config-if-range)#switchport mode access
    - S2(config-if-range)#switchport access vlan 10
    - S2(config-if-range)#exit
    - S2(config)#interface range fastEthernet 0/18-24
    - S2(config-if-range)#switchport mode access
    - S2(config-if-range)#switchport access vlan 20

- Delete the router configuration and reload it.

    - R1#erase startup-config
    - R1#reload
! Configure the aggregation interface on R1
    
    + R1(config)#interface fastEthernet 0/1
    + R1(config-if)#no shutdown
    + R1(config-if)#exit
    + R1(config)#interface fastEthernet 0/1.1
    + R1(config-subif)#encapsulation dot1Q 1
    + R1(config-subif)#ip address 172.20.1.1 255.255.255.0
    + R1(config-subif)#exit
    + R1(config)#interface fastEthernet 0/1.10
    + R1(config-subif)#encapsulation dot1Q 10
    + R1(config-subif)#ip address 172.20.10.1 255.255.255.0
    + R1(config-subif)#exit
    + R1(config)#interface fastEthernet 0/1.20
    + R1(config-subif)#encapsulation dot1Q 20
    + R1(config-subif)#ip address 172.20.20.1 255.255.255.0
    + R1(config-subif)#exit
    + R1(config)#interface fastEthernet 0/1.30
    + R1(config-subif)#encapsulation dot1Q 30
    + R1(config-subif)#ip address 172.20.30.1 255.255.255.0
    + R1(config-subif)#exit
    + R1(config)#interface fastEthernet 0/1.99
    + R1(config-subif)#encapsulation dot1Q 99 native
    + R1(config-subif)#ip address 172.20.99.1 255.255.255.0

! Configure the server's network interface on R1.

    + R1(config)#interface fastEthernet 0/0
    + R1(config-if)#ip address 172.20.50.1 255.255.255.0
    + R1(config-if)#no shutdown

- Display the switching table of Switch 2.
    - S2#show mac-address-table
```

# THANKS FOR WATCHING!!!
Contact me : https://github.com/CompanyDATV <br>
# DEMO bấm vào liên kết đó ==>
<center>
<h1>

[Video Config Topology Company Networking VLAN ](https://youtu.be/bWPsXvLX4dA) 
</h1>
<center>