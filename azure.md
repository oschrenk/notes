# Azure

Azure CLI

```
# install
brew install azure-cli

azure login
# follow the instructions on screen

# configure arm mode
azure config mode arm

# Figure out which subscription is the right one
azure account list

# Switch to that one
azure account set <id>

# Now you can interact with it
# ---------------------------

# list resource groups
azure group list

# list Network security groups
azure network nsg list <resource group>

# create a new rule for group
azure network nsg rule create <resource group> <network security group> \
testName \
--description 'testDesc' \
--protocol 'Tcp' \
--source-address-prefix '10.10.10.10/32' \
--source-port-range '*' \
--destination-address-prefix '*' \
--destination-port-range '443' \
--access 'Allow' \
--priority '4000' \
--direction 'Inbound'
```


# Connecting two Virtual Networks
**Goal**
The Mesos Cluster lives in it's own Resource Group and even more important in it's own Virtual Network. We want to be able to communicate with machines and services in the "old" infrastructure. Specifically we want
1. to send logs and metric data to the virtual machine (VM)  `AF-MONITORING`
2. we want the production and staging machines, `AF-SCALA-*` and `AF-STAGING-*` to fetch data from the `meterreading-api` service.

**Current solution**
1. We don't send logs/metrics
2. We expose `meterreading-api` as a public service, protected by HTTP Auth, and IP whitelisting

**Problems with current solution**
While the `meterreading-api` service is protected with a password and only accessible with a whitelisted IP, it still offers an attack surface, which is unnecessary. All the communications is done with services that don't need to be public to the outside world.

Outbound public traffic costs more money than traffic inside the Azure infrastructure. I don't have the volume of our outbound traffic, but let's assume some numbers
* 10 GB: 0.37 € (yes, thirty seven cents)
* 95 GB: 7.03 €

As we are closer to 10GB then 95GB, I think the cost issue is negligible.

There is an architectural issue, in the sense that the machines that we need to talk to need a public IP address, and that the machines need the right network security rules settings to be accessible from the outside but only from the correct IP-address.

This leaves to me the problem of security, and clean and simple architecture.

**Solution Space**
There are three different ways to connect two virtual networks inside Azure:
1. Use public IP adresses to interchange data via internet
2. Reuse the existing virtual network
3. VPN Gateway
4. Virtual Network Peering

## Use Public IP addresses
Add network security rules to allow public access to `AF-MONITORING`. Figure out public IP address of that machine and use it to send data from `sparkplug` and `meterreading-api` to that IP
No changes needed for `meterreading-api`, as this is already done.
### Advantages
Easy. Quick.
### Disadvantages
* We poke another hole into our infrastructure.As Influx DB and Graylog are currently not secured, other then a password to access the respective administration.
* We can't use internal DNS to route to AF-MONITORING, but rely on public IP address.
### Requirements
None.

## Reuse the existing virtual network
It should be doable to re-raise the cluster and attaching it to the existing virtual network but specifying the subnets and adresses for the nodes. Then we can add security rules that work on subnets. This needs changes inside the template. We need to re-create our clusters so we don't have clashing addresses.

### Advantages
We can close the public holes and we have only intra-infrastructure communication.
We can re-use knowledge because then everything falls back to network security group management.
### Disadvantages
It might be confusing to mix "old" infrastructure with new one, if only by the virtual network.
### Requirements
The subnets can't share subnets with the same IP address ranges.
### Risk
Not 100% that we can attach the cluster to an existing virtual network. But this is testable.

## VPN Gateway
We need to create a dynamic VPN Gateway (VNet-to-VNet) that connects the virtual network of the Mesos cluster with the virtual network of the "old" infrastructure. There is a tutorial how to do that
[Configure a VNet-to-VNet VPN gateway connection using Azure CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-cli)
It also means that we need to re-create the clusters, so we don;t have clashing IP-address ranges. This needs changes inside the template.
### Advantages
We can close the public holes and we have only intra-infrastructure communication.
### Disadvantages
Pricing. It costs around ~120€/month (for the recommended configuration, ~25€ for the minimum).
Configuration overhead.
### Requirements
The virtual networks can't share subnets with the same IP address ranges.
### Considerations
As each virtual network can only have one VPN Gateway, there might be an issue with existing gateways, especially static gateways.

## Virtual Network Peering
You can bring two virtual networks on the same level and it doesn't need a gateway. It's called Virtual Network Peering. Here's the tutorial:  [Microsoft Azure, Create Peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-create-peering#a-namecliacreate-peering---azure-cli).
It also means that we need to re-create the clusters, so we don't have clashing IP-address ranges.
### Advantages
Cheaper than a full blown VPN gateway. Only pay bandwidth. No VPN Gateway configuration
### Disadvantages
More of a consideration and it can actually be advantageous but there is no transitive peering.
### Requirements
* The peered virtual networks must exist in the same Azure region.
* The peered virtual networks must have non-overlapping IP address spaces.
* Address spaces cannot be added to, or deleted from a virtual network once a virtual network is peered with another virtual network.
* Virtual network peering is between two virtual networks. There is no derived transitive relationship across peerings. For example, if virtualNetworkA is peered with virtualNetworkB, and virtualNetworkB is peered with virtualNetworkC, virtualNetwork A is not peered to virtualNetworkC.
* At least one virtual network must be created through  the  Resource Manager deployment model

## Terminology
### Generic
**VPN** A virtual private network (VPN) consists of multiple remote end-points (typically routers, VPN gateways of software clients) joined by some sort of tunnel over another network, usually a third party network. Two such end points constitute a 'Point to Point Virtual Private Network' (or a PTP VPN). Connecting more than two end points by putting in place a mesh of tunnels creates a 'Multipoint VPN'.

**Route-Based** You get an interface much like a tunnel interface that you can route traffic through. Route-based VPNs depend on a tunnel interface specifically created for forwarding packets.
**Policy-Based** You tell the system 'every packet that matches this policy must be encrypted'.

### Azure
**Virtual Network**  A VNet is a logical isolation of the Azure cloud dedicated to your subscription. VNets are isolated from one another. You can create separate VNets for development, testing, and production that use the same CIDR address blocks.

**Network interface** A network interface (NIC) is the interconnection between a VM and a virtual network (VNet). A VM must have at least one NIC, but can have more than one,

**VPN Gateway** A VPN gateway is a type of virtual network gateway that sends encrypted traffic across a _public connection_ to an on-premises location. You can also use VPN gateways to send encrypted traffic between Azure virtual networks over the Microsoft network. Each virtual network can have only one VPN gateway, however, you can create multiple connections to the same VPN gateway. This is called a Multi-Site connection configuration.

**Static Routing VPN**  are also referred to as policy-based VPNs
 Multi-Site VPN, VNet to VNet, and Point-to-Site **are not** supported with static routing VPN gateways. Policy-based VPNs encrypt and route packets through an interface based on a customer-defined policy. The policy is usually defined as an access list. Static routing VPNs require a static routing VPN gateway.  Only 1 Site-to-Site (S2S)  is supported.
**Dynamic Routing VPN** Dynamic routing VPNs are also referred to as route-based VPNs.  A dynamic routing VPN gateway is required for Multi-Site VPN, VNet to VNet, and Point-to-Site.

**Site-to-Site** A Site-to-Site (S2S) VPN gateway connection is a connection over a over IPsec/IKE VPN tunnel. This type of connection requires a VPN device located on-premises that has a public IP address assigned to it and is not located behind a NAT.  Requires compatible VPN hardware for each on-premises location.
**Multi-Site**  Is a variation of the Site-to-Site. You create more than one VPN connection from your virtual network gateway, typically connecting to multiple on-premises sites. You must use a RouteBased VPN type (known as a dynamic gateway when working with classic VNets). Because each virtual network can only have one VPN gateway, all connections through the gateway share the available bandwidth.
**Point-to-Site** A Point-to-Site (P2S) VPN gateway connection allows you to create a secure connection to your virtual network from an individual client computer.  P2S is a VPN connection over SSTP (Secure Socket Tunneling Protocol).  P2S connections do not require a VPN device or a public-facing IP address to work. You establish the VPN connection by starting it from the client computer. This solution is useful when you want to connect to your VNet from a remote location, such as from home. P2S connections can be used with S2S connections through the same VPN gateway, as long as all the configuration requirements for both connections are compatible.
**VNet-to-VNet** Connecting a virtual network to another virtual network (VNet-to-VNet) is similar to connecting a VNet to an on-premises site location. Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE. You can even combine VNet-to-VNet communication with multi-site connection configurations.  The VNets you connect can be in the same or different regions, in the same or different subscriptions, or in the same or different deployment models.

## Resources
[Wikipedia, Virtual Network](https://en.wikipedia.org/wiki/Virtual_network)
[Microsoft Azure, Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview)
[Microsoft Azure, Virtual networks and Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/network-overview)
[Microsoft Azure, About VPN Gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways)
[Microsoft Azure, Virtual Network Peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)

### Pricing

VNET Peering, Inbound: €0.009 per GB
VNET Peering, Outbound: €0.009 per GB

VPN Gateway, Inbound & Outbound ~€119.21/month, 500 Mbps

Public, Inbound: Free
Public, Outbound: First 5GB/month are free, to 10TB €0.074 per GB (to same Zone)

[Microsoft Azure, Bandwidth Pricing Details](https://azure.microsoft.com/en-us/pricing/details/bandwidth/)
[Microsoft Azure, Virtual Network Pricing](https://azure.microsoft.com/en-gb/pricing/details/virtual-network/)
[Microsoft Azure, VPN Gateway Pricing](https://azure.microsoft.com/en-gb/pricing/details/vpn-gateway/)
