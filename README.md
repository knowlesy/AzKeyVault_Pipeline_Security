# AzKeyVault_Pipeline_Security

Enabling access to your Key Vaults securely with Azure Devops Library Sets

## KeyVaults




Azure Key vaults enable you to do the following 

* **Secrets Management** - Azure Key Vault can be used to Securely store and tightly control access to tokens, passwords, certificates, API keys, and other secrets
* **Key Management** - Azure Key Vault can be used as a Key Management solution. Azure Key Vault makes it easy to create and control the encryption keys used to encrypt your data.
* **Certificate Management** - Azure Key Vault lets you easily provision, manage, and deploy public and private Transport Layer Security/Secure Sockets Layer (TLS/SSL) certificates for use with Azure and your internal connected resources.

However they also enable auditing, RBAC (Access policies or Azure RBAC) along with setting restricted access via firewall / network access control lists.

In this we will go over the 3 options for securing access to your keyvault when using lirary sets in Azure Devops Pipelines 

I have created 3 Key Vaults with varying degrees of access all controlled via the networking section 

The Key Vaults consist of the following

* Configured with Azure RBAC
* Have a test secret in them
* An SPN "KeyVault" as reader to the subscription and as "Key Vault Secrets User" (read in Key Vaults) to the Key Vault
* My own account added as a Key Vault Secrets Officer (to create / edit / read)

The SPN "Key Vault" was added to my test project as a connector in ADO 


![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/9d681e8f-d9c1-4ad1-9e36-dc56011d39a1)

## Network Config Options

### IP MSOnly  
Network settings
![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/9c5265cb-a606-42a5-9f53-e0f9ae7ee1a5)

ADO View in Library sets 
![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/bb176497-a8f6-4298-b9c4-6ec9cd712350)

My access via web page

![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/7e94d6d2-23e0-40e1-9474-8df3da09c888)


You would use this private endpoints - ADO unable to access this

### IP Public 
Network settings
![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/1964f2f1-6998-46af-840e-0fa177e494d5)

ADO View in Library sets 
![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/0e382900-5863-424b-ad78-1b00363bbeaa)

My access via web page

![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/e8f4a1a2-e8e5-40d0-a99f-64ea643d8706)


Could be utilised for a variety of things (think IP address changes)  however not ideal if a service principle or users account is compromised

### IP Filtered 
Network settings
![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/9bb7f96f-8296-41d9-8017-67b5ad0eabc3)

ADO View in Library sets 
![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/b0e36d1a-8bb8-48e6-ab9b-3572dd268698)

My access via web page

![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/fd6b4b8e-98a9-43d7-a658-b0e21a46896d)


If you require restricted access to a on prem or Azure network (that isnt set up to utilise private link) as well as public access (ADO Access), this would be the best way  to enable that. 

## Giving ADO Access 
As you can see in the IP Filtered it has access with the single range, if you visit the link below in further reading "Allowed IP addresses and domain URLs - INBOUND" this will provide you with all the regions ADO works in and their corresponding IP addresses.

To find your region go to your Organisation in ADO > Click Organization settings > In Overview you will see region for me its UK South

![image](https://github.com/knowlesy/AzKeyVault_Pipeline_Security/assets/20459678/91fc1aed-5bf7-4dbc-8883-8e5a6b1c3e4a)

Note - Review any other IP address you may require 

### Further Reading 
* [Allowed IP addresses and domain URLs - INBOUND](https://learn.microsoft.com/en-us/azure/devops/organizations/security/allow-list-ip-url?view=azure-devops&tabs=IP-V4#inbound-connections)
* [Azure Keyvaults](https://learn.microsoft.com/en-us/azure/key-vault/)
* [Key Vault Best Practises](https://learn.microsoft.com/en-us/azure/key-vault/general/best-practices)
* [Self Hosted Agents](https://learn.microsoft.com/en-gb/azure/devops/pipelines/agents/windows-agent?view=azure-devops)
* [What is IP Address of Azure DevOps Build Agent and Set Firewall to Allow it](https://build5nines.com/what-is-ip-address-of-azure-devops-build-agent-and-set-firewall-to-allow-it/)
* [Configure Azure Key Vault firewalls and virtual networks](https://learn.microsoft.com/en-gb/azure/key-vault/general/network-security?WT.mc_id=Portal-Microsoft_Azure_KeyVault)
* [Use Azure Key Vault secrets in Azure Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault?view=azure-devops&tabs=yaml)
