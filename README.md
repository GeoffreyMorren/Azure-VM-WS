<div align="center">

# Project
With this project, the goal is to create a virtual machine and deploy a web server. Which we will accomplish by completing the following tasks:
<li>Create a Resource Group</li>
<li>Create a Virtual Network and a subnet</li>
<li>Protect a subnet using a Network Security Group</li>
<li>Deploy Bastion to connect to a Virtual Machine</li>
<li>Create an Ubuntu Server Virtual Machine</li>
<li>Install Nextcloud by connecting via SSH using Bastion</li>
<li>Publish an IP</li>
<li>Create a DNS label</li>

## Create a Resource Group
In the overview, select create a resource. Search for “resource group”, followed by Create. 

![1](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/466dc557-0b47-4b75-b04e-2b5bf167ed91)

After filling in the name of the resource group (RG-USE-Nextcloud), click Review + create, followed by Create. 

![2](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/dffb5b5c-c53b-4551-b4ca-b809e36facfe)

A confirmation message will appear on the top right corner. Via the notification button, we access the newly created resource group. 

![3](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/8b0c030d-6283-49d5-9a01-9fc83fb5e99c)

The final result will look like this.

![4](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/45635677-ccee-4bb4-bac2-d712e73787aa)

## Create a Virtual Network and a Subnet
To create a virtual network, go to the newly created resource group and click Add. 

![5](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/54735013-4933-4f44-b227-fed3e5f09154)

Search for “virtual network” and click Create. 

![6](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/7c9fa66a-21d1-4786-965c-556ce77225d1)

The resource group will be automatically selected this way. Under Instance Details, fill in the name of the virtual network (VNET-USE-Nextcloud). Note: the virtual machine needs to be in the same region as the virtual network. Lastly, select Next: IP Addresses. 

![7](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/ee310fba-8c0d-4042-826c-3dea1e24e3b3)

A default IPv4 address space and subnet are already filled in. Delete these and fill in your own IPv4 address (172.10.0.0/16) space followed by Add subnet. Fill in the subnet name (SNET-USE-Nextcloud) and subnet address range (172.10.0.0/24). 

![8](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/33c502de-ca13-4e47-8bf3-1546a8fa90c1)

Finalize this step by clicking Review + create, followed by Create. 

![9](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/9aeb4905-049e-4530-8044-9a875d449f63)

When the Deployment has succeeded, navigate back to the resource group. 

![10](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/345b4c44-b887-435f-baa6-61717c01bd2d)

Wait or refresh the page until the virtual network appears in the resource group. Select the virtual network. 

![11](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/e0e250c9-3392-4bf8-b9b4-5cb825a6b1d8)

Navigate to the side menu, under settings, select Address space. Review and confirm the selected address space and range. 

![12](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/33a574a2-2f10-43a6-99e3-677d7ef50ad7)

Navigate back to the side menu, under settings, select Subnets. Click on the subnet name to display the subnet information. 

![13](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/adef2ed5-6265-4408-853c-2d9cb0c7c032)

Navigate to the side menu, under Monitoring, select Diagram to get an overview of the current network topology.

![14](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/56503276-5cf9-49a3-b438-65838a73380f)

## Protect a Subnet Using a Network Security Group
A Network Security Group will allow us to filter inbound and outbound traffic. Navigate to the resource group and click Add. 

![15](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/ba002061-2096-4528-8d74-e6aa3735a1eb)

Search for and select “network security group”. Click Create. 

![16](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/6b1a415a-db78-4988-aec3-2aa4e493f2ea)

The resource group is automatically selected. Under Instance Details, fill in the name of the Network Security Group (NSG-USE-Nextcloud). Continue by clicking Review + create and create. 

![17](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/75dff53e-d719-4a36-b412-e43ec28cf1b9)

After the deployment has succeeded, click go to resource. 

![18](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/c2467a6a-9f66-47fb-a4c5-4971e14a35f1)

Some inbound and outbound rules are automatically created. To start assigning rules to our subnet, navigate back to the resource group. 

![19](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/ea3fd1ba-c6dd-4f19-961f-8d9a57c5d6c9)

The Network Security Group will now be added under resources. We continue by going to the virtual network. 

![20](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/395254f1-eb0a-4134-8862-35474516d877)

Navigate to the side menu, under Settings, select Subnets. Click on the subnet to display the subnet information. Next, click the dropdown menu under Network Security Group. Select the resource group: RG-USE-Nextcloud and save. 

![21](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/78755b7f-eda7-4970-8dad-db818a170da6)

Navigate to the side menu, under Monitoring, select Diagram to see that the subnet is now protected by the Network Security Group.

![22](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/bed906cc-ce66-4842-a9c4-9cd1499a3879)

## Deploy Bastion to Connect to a Virtual Machine
To connect Bastion to a virtual machine, we need to create a subnet for it first. Navigate to the side menu, under settings, select Subnets and add a subnet. Fill in the name for the subnet (AzureBastionSubnet) and leave the address range unchanged. Click Save. 

![23](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/60ac8418-f888-4578-b8d9-a171158f952d)

When the subnet has been successfully added, it will appear in the subnets section. Navigate to the side menu, under Monitoring, select Diagram to see the AzureBastionSubnet added to the virtual network. 

![24](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/53cdf5d5-1df7-42da-bacb-5d1d6c7cad99)

Navigate to the side menu, select Overview, click on the resource group, then Add. In the search field, type and select “bastion”, followed by create. The resource group is automatically selected. Fill in the name under Instance details (BASTION-USE-Nextcloud), select the Virtual network (VNET-USE-Nextcloud). The AzureBastionSubnet will be automatically selected. Under Public IP address, edit the Public IP address name (BASTIONIP-USE-Nextcloud). Finally, click Review + create, followed by create. 

![25](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/c4a8b010-9911-4c95-a91c-6c70359a3ead)

The deployment of Bastion might take a while to complete. When completed, the Overview selection will look like this.

![26](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/6580030d-2438-4b55-b974-622cbc170c11)

## Create an Ubuntu Server Virtual Machine
In the resource group, click Add, select the Ubuntu Server 18.04 LTS virtual machine. The resource group will be automatically filled in. Under Instance details, fill in the name (VM-USE-Nextcloud). For the region and availability zone, it is important to use the ones related to the virtual network. Lastly, select a size, which can be changed by clicking See all sizes if needed. 

![27](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/9631b655-2de5-44d9-8146-7d03ca9b3a3e)

In the Administrator account section, we keep the default Authentication type on SSH public key. Next, we can change the Username and Key pair name (VM-USE-Nextcloud) if needed. Since we don’t want the SSH port to be available on the internet, we set the Public inbound ports to None. We continue by selecting Next: Disks, leaving the default settings on the Disks tab, and continue to Next: Networking. 

![28](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/0edb1325-f301-46b9-8281-2944cc2e6e22)

In the Networking tab, we will change the Public IP to None since we want to set the IP manually later. We leave all other options as default and click Review + create, followed by Create. 

![29](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/9193e85e-3450-4a6f-bda8-86453c8fc7a3)

There will be a pop-up that will allow us to download a new key pair that we can store on our device. 

![30](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/31a9c2ee-1c9b-4975-8124-bd0daefdaa83)

After a few minutes, the deployment should be successful.

## Install Nextcloud by Connecting via SSH Using Bastion
To get started, we navigate back to our resource group and select our virtual network (VNET-USE-Nextcloud). In the side menu, under monitoring, select Diagram to see the current network topology. 

![31](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/8db3c0d7-7982-467c-99f6-2621c179f9dc)

We continue by selecting our virtual machine (VM-USE-Nextcloud) to see its resource details. We take note of the Private IP address 172.10.0.4. Selecting Connect, then from the options listed, select Bastion. 

![32](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/e241e6f6-a9ed-4673-be7d-4cfd4e95a0eb)

On the following page, select Use Bastion. Fill in the username set earlier and for the Authentication type select SSH Private Key from Local File. Navigate to the stored private key, followed by Connect. The virtual machine will be opened in another tab. For the Nextcloud installation, type the following command: sudo snap install nextcloud 

![33](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/6c6241f6-2776-4d12-ad12-5b8738c5f507)

Next, an admin user will have to be created. We use the following command: sudo nextcloud.manual-install admin “username” 

![34](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/7ce54a1c-a8ef-4377-ad3b-59bb9f19f2fe)

Finish by generating a key and self-signed certificate by using the following command: sudo nextcloud.enable-https self-signed.

![35](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/b0d3aa80-130a-4a53-98ae-2bac45425820)

## Publish an IP
While still in the virtual machine resource, navigate to the side menu, under settings, select Networking. Click on the Network Interface. 

![36](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/62684697-e83f-44e8-840a-9b674b9b64d4)

Next, navigate to the side menu, under settings, select IP configurations. We will see our config, which has no Public IP address assigned. To do this manually, click on the ipconfig1 line. 

![37](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/9074db8e-f6c5-4742-883c-a90e8e8f8850)

Set Public IP address to Associate, click Create new. Fill in a name for the public IP address (VMIP-USE-Nextcloud), set the SKU to Standard and click OK. 

![38](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/7f957510-b281-47a9-9f7a-fa8cb8afd2eb)

Save the configuration. When the public IP address has completed successfully, navigate back to the virtual machine (VM-USE-Nextcloud) and click on Overview to verify that a Public IP address has been assigned under the Networking section. 

![39](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/cfe1efb7-fff9-42e3-ac4c-061dba30889a)

Copy the Public IP address and open it in a different tab in your browser. Likely, this is not yet working due to the fact that no rule is set in place to allow inbound traffic. Back on the virtual machine tab, navigate to the side menu, under Settings, select Networking. Click Add inbound port rule. 

![40](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/3d348f79-00ca-4b23-9c18-0a9571c1a174)

For the source, fill in your IP address. For Destination, fill in the Public IP address. For the Service, select HTTPS. Make sure the Action is set to Allow and fill in a name for the rule (HTTPS_Nextcloud). Click Add to finish. 

![41](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/da119f91-8231-4078-82c1-be0156008da6)

Now, go back to another tab in your browser. Browse to the Public IP address. Because of the self-signed certificate, a warning message will appear. This can be ignored. Continue to the server.

![42](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/41223726-df4b-4f39-903d-ac41a11b7efb)

## Create a DNS Label
Start on the virtual network page. Navigate to the side menu, under Monitoring, select Diagram to see the current Network topology. Notice that a public IP address is assigned to our virtual machine. Click on the public IP icon. 

![43](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/29167f43-d719-4ef4-a31e-4dc0c215fd47)

Navigate to the side menu, under Settings, select Configuration. Notice the DNS name label option. When filling in a DNS that is already taken, Azure will display an error message. Choose a domain name that is not yet taken, which will include (in this case) the .eastus.cloudapp.azure.com extension. Click on Save to complete the configuration. 

![44](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/cc0c0ee0-f924-45b2-b22a-5b53127e4a26)

Go back to Overview, click on the resource group and then select the virtual machine. Under the Networking section, verify that the DNS is now set. Connect to the virtual via Bastion like we did before. Use the following command to communicate the DNS label to Nextcloud: sudo nextcloud.occ config:system:set trusted_domains 1 --value=DNS. If all went well, you’ll get a confirmation message: 

![45](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/fabae7d1-8ff7-49f8-af90-5ce214931c3c)

Exit the SSH connection and close Bastion. Open a new tab in your browser and browse to your DNS. Again, accept the self-signed certificate. You should be able to log in to your Nextcloud server.

![46](https://github.com/GeoffreyMorren/Azure-VM-WS/assets/152500568/9f6061f2-5d89-4b7d-9311-ee3c25dac9e2)

</div>
