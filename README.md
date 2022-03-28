# tfc_private_module_azure_random_pet

This repository describes on how to create a repository in Azure DevOps and use this repository in Terraform Cloud Private registry as a module.

Steps involved:
- Create repository within Azure DevOps
- Create a VCS connection to Azure DevOps in Terraform Cloud
- Add the repository as a Private Module
- Use the Private module with Terraform CLI

# Prerequisites

## Login account for the following 
- Azure DevOps login
- Terraform Cloud login

## Install terraform  
See the following documentation [How to install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

## Azure VCS connection from Terraform Cloud
Have a VCS connection to Azure from you Terraform Cloud organization. [Example how to](https://github.com/munnep/tfc_azure_vcs_connection/blob/main/README.md#create-a-vcs-provider-to-azure-devops)

# How to

## Azure DevOps
- Login to Azure DevOps portal
[https://dev.azure.com/](https://dev.azure.com/)
- create a new project    
![](media/2022-03-28-11-34-25.png)  
- create the project `terraform-random-pet_azure`  
![](media/2022-03-28-11-40-17.png)  
- Create a repository    
![](media/2022-03-28-11-41-32.png)   
![](media/2022-03-28-11-41-41.png)  
- create a new branch    
![](media/2022-03-28-11-42-13.png)  
![](media/2022-03-28-11-42-43.png)  
- create a new file called `main.tf`.         
![](media/2022-03-28-11-43-49.png)  
```
resource "random_pet" "pet" {
    prefix = var.prefix
}
```
- create a new file called `variables.tf`.    
![](media/2022-03-28-11-45-11.png)  
```
variable "prefix" {
    type = string
    description = "prefix for the name of your pet"
    default = "blabla"
}
```
- create a new file called `outputs.tf`
```
output "pet" {
  value = random_pet.pet.id
}
```
- alter the `README.md`
```
# Private module terraform-random-pet_azure

This module will create a random name for you pet_azure

default prefix of the name is `blabla`

change the variable `prefix` in calling this module for a different prefix name
```
- Make a pull request    
![](media/2022-03-28-11-49-35.png)  
![](media/2022-03-28-11-49-49.png)  
- Give it a title   
![](media/2022-03-28-11-51-00.png)  
- Click on complete    
![](media/2022-03-28-11-51-30.png)    
![](media/2022-03-28-11-51-43.png)  
- Create a tag on the `main` repostitory      
![](media/2022-03-28-11-52-51.png)  
- Create a tag `0.0.1`.     
![](media/2022-03-28-11-53-40.png)  

## Terraform Cloud

- Login to Terraform Cloud
- Go to registry -> modules    
![](media/2022-03-28-11-55-35.png)  
- Publish a module      
![](media/2022-03-28-11-55-55.png)  
- Click on your Azure DevOps connection you have    
![](media/2022-03-28-11-56-27.png)  
- Select your module  
![](media/2022-03-28-11-56-50.png)  
- Publish module  
![](media/2022-03-28-11-57-11.png)  
- Your module should now be available for use    
![](media/2022-03-28-11-58-17.png)   
- Example usage code
```
module "pet_azure" {
  source  = "app.terraform.io/patrickmunne/pet_azure/random"
  version = "0.0.1"
}
```

