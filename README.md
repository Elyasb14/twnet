# TWNET

The network architecture and documentation for the tinyweather network. 

## Overview

It all basically comes down to a [wireguard](wireguard.org) VPN. Nodes will be "peers" and will tunnel to "core" routers where they can gain access to proxy and tsdb servers. 

## Wireguard

Each vpn tunnel between the first "core" router and a node will be defined by a network in the address space `10.0.3.0/24`. If we ever get another core router, we can define another subnet of `10.0.4.0/24` and so on. Each wireguard tunnel will be created with a "peer" with address `10.0.3.x/32`. This peer will be the node. Below is a description of what that process looks like in practice. Lets say we have router1 and node1. Router1 has a wireguard interface with a private and public key, and a port it listens on. Router 1 also has an address of `10.0.3.1/24` assosciated with the wireguard interface. Node 1 will be a wireguard peer with a public and private key and an address of `10.0.3.2/32`. On node1, there will be a configuration file that looks something like the following:


```
[Interface]
PrivateKey = <node1-privatekey> 
ListenPort = 14008
Address = 10.0.3.2/32
DNS = 1.1.1.1

[Peer]
PublicKey = <router1-publickey> 
AllowedIPs = 0.0.0.0/0
Endpoint = <router1-ip>:13231
PersistentKeepalive = 25
```

On the router1 side, there will be a wireguard peer that looks like the following:

```routeros
[admin@home-tik] > /interface/wireguard/peers/print
Columns: INTERFACE, PUBLIC-KEY, ENDPOINT-PORT, ALLOWED-ADDRESS
# INTERFACE   PUBLIC-KEY                                    ENDPOINT-PORT  ALLOWED-ADDRESS
;;; tw-test
0 wireguard1  <node1-publickey>=          14008 10.0.3.2/32 
[admin@home-tik] > /interface/wireguard/peers/print
```
