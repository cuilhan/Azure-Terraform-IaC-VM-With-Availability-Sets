- What does do this Terraform Project?

This terraform project uses AzureRM provider from Terraform Registry. We use AzureRM provider for creating, modifying or destroying our infrastructures  on Azure.
We have region, vm admin password and frontend instances variables that are located in variables.tf file.

- variables.tf file
Have default hardcoded configurations that will be used for plan and apply operations. arm_region is our default Azure region that will be location to our created resources.

- backend.tf file
It's used to store our terraform state file in azure storage account container. We doing this for not losing our "tfstate" file an for security reasons. Also any other permitted colleagues can use our state file for collaboration.

terraform {
    backend "azurerm" {
        storage_account_name = "umitilhanstorageaccount" # Use your own unique name here
        container_name       = "terraform-sample"        # Use your own container name here
        key                  = "terraform.tfstate"       # Add a name to the state file
        resource_group_name  = "UmitIlhanRG"         # Use your own resource group name here
    }
}

- output.tf file
This file is used for storing our outputs for using later when we need it. It outputs frontend id, backend id, dmz id and load balancer ip address.


- virtual-machines.tf
We see Availability Sets configured for high availability scenarios. Fault domain number is set to 3 and update domain number is set to 20.
VM is uses "Ubuntu 16.04 LTS" version and has os and data disks that they are configured as destroy on vm termination. Our data disk is have 1023GB size and its locally redundant storage. We see VM size is "Standard_DS1_v2".
Our vm also have post installation configuration (desired state configuration). Its allows we can use SSH password for login not just SSH keys.

- load-balancer.tf
Our load balancer configuration has dynamic private ip address allocation which is good. Because in case of ip address of vm is changed we don't need to take an action. It will be updated automatically.
Load balancer has health probes which is uses http 80 and https 443 port they are uses TCP protocol. We have a pool that contains load balancers for balancing loads between machines.

- vnet-subnet.tf
VNet region is will be located to East US because it's in our variables.tf file. We see our vnet is configured to "10.0.0.0/16" address space. 
Frontend subnet is configured to 10.0.1.0/24 address space.
Backend subnet is configured to 10.0.2.0/24 address space.
DMZ subnet is configured to 10.0.3.0/24 address space.
This means we dont have any collusion on our network configuration.


