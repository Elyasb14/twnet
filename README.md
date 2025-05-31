# TWNET

The network architecture and documentation for the tinyweather network. 

## Overview

It all basically comes down to a [wireguard](wireguard.org) VPN. Nodes will establish vpn tunnels with "core" routers which will be connected to proxy and tsdb servers. Below we will describe private ip address allocations for nodes, proxy servers, tsdb servers, wireguard interfaces, etc.... 

# Allocations

## Wireguard

Each vpn tunnel between the first "core" router and a node will be defined by a network in the address space `10.0.3.0/24 subnet`. If we ever get another core router, we can define another subnet of `10.0.4.0/24` and so on. Each wireguard tunnel will be a subnet of `10.0.3.0/24` with the template `10.0.3.x/30` with the high address being the node side and the low address being the router side. So as an example

| Test Network|  `10.0.3.0/30`|
| -------- | ------- |
| Network addr | `10.0.3.0`|
| Core router addr | `10.0.3.1` |
| Node addr | `10.0.3.2` |
| Broadcast addr | `10.0.3.3` |

## Loopback addr

Each of the core routers need a loopback address for dynamic routing reasons. These will come from the address space `
