# DEPLOYING-AN-AZURE-P2S-VPN-USING-ENTRA-ID-AS-AUTHENTICATION

In this lab, we will be setting up and configuring Azure point to site VPN using Entra ID (Active Directory) as the authentication.
In accomplishing this, i had my Azure account ready to deploy the resources that will enable me in setting the VPN Gateway.
Resources to deploy are :-
1) A resource group
2) A virtual network  and a gateway subnet
3) A Virtual Network Gateway (VNG)
4) A Standard Public IP (This is done when creating the VPN gateway)
   
We already have a Windows VM (Virtual Machine) deployed, an Azure VPN client application downloaded from Microsoft store also we have created a user with a Global Admin role in our Entra ID (Active Directory) to be able to authorize the Azure VPN aaplication.


## SETTING UP THE RESOURCE GROUP, VIRTUAL NETWORK AND GATEWAY SUBNET

In the first picture, we deployed the resource group; the resource group serves as a container for oher resources in Azure.

![P2](https://github.com/user-attachments/assets/236b0936-6ceb-47d0-8cb2-d64708e6e876)

In this next section, we deployed a Virtual Network with a CIDR range of 10.10.0.0/16 and also created a gateway subnet with a CIDR range of 10.10.1.0/24.

![P3](https://github.com/user-attachments/assets/f3df3321-513f-4eb6-b5cf-422ab578cc3e)

Subnets enable you to segment the virtual network into one or more sub networks and allocate a portion of the virtual network's address space to each subnet. You can then deploy Azure resources in a specific subnet. Just like in a traditional network, subnets allow you to segment your virtual network address space into segments that are appropriate for the organization's internal network. Azure has a dedicated subnet for some resources e.g. Gateway subnet for VNG, Firewall subnet for Azure Firewall, Bastion subnet for the Bastion resource, Firewall management subnet for forced tunelling and the Route server subnet.

![P4](https://github.com/user-attachments/assets/659643f5-ad33-403a-9cdd-fef4495476a0)

![P5](https://github.com/user-attachments/assets/35b032b2-43ed-4d86-b576-ade587178966)

## DEPLOYING A VPN GATEWAY
An Azure VPN gateway is a specific type of virtual network gateway that is used to send and receive encrypted traffic between an Azure virtual network and an on-premises location over the public Internet. Azure VPN gateways can also be used to connect separate Azure virtual networks using an encrypted tunnel across the Microsoft network backbone. For further readings you can check out the Microsoft public docummentation https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways

In this lab we will be configuring a VPN gateway for a point-to-site. 
Point-to-site VPN connection: A point-to-site VPN connection can be used to connect a single computer to an Azure virtual network. A P2S connection is established by starting it from the client computer. This type of VPN connection is commonly used by remote workers with portable computers. About Azure P2S https://learn.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about

VPN Gateway pass validation below;

![P6-VNG-VALIDATION](https://github.com/user-attachments/assets/35c07486-6eba-4c7d-9147-fd8fbceb2167)

The picture below shows the VPN gateway created alongside its Public IP (20.254.243.78 (ugo-VNG-Public-IP))
![P7-VNG-created](https://github.com/user-attachments/assets/86e04fe1-eef6-4de4-9a59-a38bba69c298)

Since Active Directory was used as the authentication type, i had to use the Global Admin role to grant consent to the Azure VPN app registration. This allows the Azure VPN application to sign in and read user profiles. I copied and paste the URL that pertains to the deployment location in the address bar of my browser:

 Public URL used to grant access https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent

Reference: https://learn.microsoft.com/en-us/azure/vpn-gateway/openvpn-azure-ad-tenant#authorize-the-azure-vpn-application

The picture below shows the confirmation for the access been granted

![access grant](https://github.com/user-attachments/assets/ffc75f8f-5f7b-4c98-9b10-a5e415bfd666)


![P9 Enterprise Application](https://github.com/user-attachments/assets/7f5a2f28-510f-4f7c-a70c-c76ea4a37d2d)


In this next section, i proceeded with configuring the point-to-site where an address pool of 172.16.8.0/24 was used, OPEN-VPN (SSL) was the tunnel type since we intend in using Entra ID (Active Directory) as the authentication type. *Circled 1* is the Tenant ID which can be found in the Entra ID overview page https://login.microsoftonline.com/ tenant-ID /, *Circled 2* is the audience which was gotten from the enterprise application, *Circled 3* was gotten by the addition of https://sts.windows.net/ Tenant-ID / and then i saved the configuration.

![P7-P2S-Config](https://github.com/user-attachments/assets/edcfa4bf-8b7a-4b15-8095-a0ccbed41fd4)


### DOWNLOADING THE AZURE VPN CLIENT PROFILE CONFIGURATION PACKAGE, CONFIGURING AND CONNECTING.

In this section, I generated and downloaded the Azure VPN Client profile configuration package. This package contains the settings that i used to configure the Azure VPN Client profile on my computer.
At the top of the Point-to-site configuration page, i Downloaded the VPN client, extracted the downloaded zip file, Browsed to the unzipped "AzureVPN" folder.
Opening the Azure VPN Client, I selected the + on the bottom left of the page, then select Import. As shown in the picture below

![VPN Client](https://github.com/user-attachments/assets/cd58f086-5993-43be-94e1-3fa060f21263)


I browsed to the Azure VPN Client profile configuration folder that i extracted. Opened the AzureVPN folder and selected the client profile configuration file.

![VPN Client 2](https://github.com/user-attachments/assets/176dab04-12fb-44f9-bc8e-68fbe39a5a05)

The picture below shows the green icon, successful connection and the user client IP

![Successful](https://github.com/user-attachments/assets/21aa19ba-746b-4e29-986a-ec438beb6ff5)


In the picture below, we can see on our command prompt, 1 - the default gateway IP and 2 - the user client IP (my device vpn ip)

![IPconfig](https://github.com/user-attachments/assets/753b2e22-7213-4190-b6ae-d8e87bba8569)































