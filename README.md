# DEPLOYING-AN-AZURE-P2S-VPN-USING-ENTRA-ID-AS-AUTHENTICATION

In this lab, we will be setting up and configuring Azure point to site VPN using Entra ID (Active Directory) as the authentication.
In accomplishing this, i had my Azure account ready to deploy the resources that will enable me in setting the VPN Gateway.
Resources to deploy are :-
1) A resource group
2) A virtual network  and a gateway subnet
3) A Virtual Network Gateway (VNG)
We already have a Windows VM (Virtual Machine) deployed alongside an Azure VPN client application downloaded from Microsoft store.

# SETTING UP THE RESOURCE GROUP, VIRTUAL NETWORK AND GATEWAY SUBNET

In the first picture, we deployed the resource group; the resource group serves as a container for oher resources in Azure.


In this next section, we deployed a Virtual Network with a CIDR range of 10.10.0.0/16 and also created a gateway subnet with a CIDR range of 10.10.1.0/24.
Subnets enable you to segment the virtual network into one or more sub networks and allocate a portion of the virtual network's address space to each subnet. You can then deploy Azure resources in a specific subnet. Just like in a traditional network, subnets allow you to segment your virtual network address space into segments that are appropriate for the organization's internal network. Azure has a dedicated subnet for some resources e.g. Gateway subnet for VNG, Firewall subnet for Azure Firewall, Bastion subnet for the Bastion resource, Firewall management subnet for forced tunelling and the Route server subnet.


# DEPLOYING A VPN GATEWAY
An Azure VPN gateway is a specific type of virtual network gateway that is used to send and receive encrypted traffic between an Azure virtual network and an on-premises location over the public Internet. Azure VPN gateways can also be used to connect separate Azure virtual networks using an encrypted tunnel across the Microsoft network backbone. For further readings you can check out the Microsoft public docummentation https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways

In this lab we will be configuring a VPN gateway for a point-to-site. 
Point-to-site VPN connection: A point-to-site VPN connection can be used to connect a single computer to an Azure virtual network. A P2S connection is established by starting it from the client computer. This type of VPN connection is commonly used by remote workers with portable computers. About Azure P2S https://learn.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about



