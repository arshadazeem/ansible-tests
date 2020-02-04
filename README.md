**Ansible:** Ansible is an Infrastructure Automation tool used to provision and manage Infra on prem and in the cloud

**How Ansible works?**
	• Ansible has a central node, and it works by pushing Ansible Modules onto target servers/nodes (called "Inventory") where the configuration has to be applied
	• Ansible does not use Agents (*It rather uses SSH to manage target nodes). Hence it is light weight
	• The inventory (target machines that need to be managed) is specified in a INI or yaml config files.
	• Ansible central node communicates with target nodes via SSH
	• All target servers/nodes that need to be managed are listed as inventory in a config file or inventory file
	• Ansible uses yaml for configuration

**Ansible Playbook**
Ansible Playbooks are scripts that provision or manage Infrastructure through Ansible
Playbooks work in conjunction with Roles, but we don't need Roles to run playbook (Roles are needed for code reuse, modulariation etc.)

**Ansible Roles**
	• Roles are reusable "IAC components" that can be imported into Playbooks
	• They are not IAM roles in that sense, but rather reusable code snippets to perform an operation such as provisioning Infra, deploying apps etc.
	• They are not playbooks, but modularized components that follow a particular dir structure

**Ansible Modules**
Ansible modules are reusable, standalone scripts that can be used by the Ansible API, or by the ansible or ansible-playbook programs. 

**SKUs - Ansible Engine vs Ansible Tower**
	• Ansible Engine is the engine/runtime that supports Ansible
	• Ansible Tower provides GUI, Management capabilities over Ansible Engine. It has support for RBAC, Job scheduling, REST API, multi-playbook workflow etc.
	• Ansible Tower uses a PostGres DB

**Supported OS**
	• Support for RHEL, CentOS, Ubuntu, MacOS and Windows (With Windows Remote Management instead of SSH)
	• Commercial Support available through Red Hat, Community edition supported through CentOS


**Ansible for Azure**
	• Ansible has modules for Azure, and also preview modules for Azure
	• We can run ARM templates for Ansible to plug any gaps for unsupported modules (just like Terraform supports ARM execution)
	• We can also write our own modules & plugins as needed
	• Ansible Tower from market place comes with pre-configured Ansible tower on RHEL

**Ansible Galaxy** - Like docker hub, it is a place (website) to download ansible roles


**Ansible on Azure**

	1. Setup Ansible following the latest docs
	2. Setup Tower 
		a. Download from RH portal, or wget following the latest docs & get a trial subscription for RH
	3. Install Ansible for Azure modules
	4. Create Service Principal in Azure (Create an App Registation, Give the app permissions to your Azure Subscription).
	5. Copy the Service Principal Client Id, Tenant Id, Azure Subscription Id, and Client Secret 

```

- sudo pip install ansible[azure]
- ansible-galaxy install azure.azure_preview_modules
- cat ~/.ansible/roles/azure.azure_preview_modules/files/requirements-azure.txt
- wget https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
- tar xzvf ansible-tower-setup-latest.tar.gz
- cd ansible-tower-setup-3.6.2-1/
- vi inventory
- sudo ./setup.sh
- ansible-tower-service status
- mkdir ansible-scripts
- cd ansible-scripts/
- vi vmss-create.yaml
   (*add playbook tasks)
- vi credentials
   (*add Service Principal Credentials)

- export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
- export AZURE_CLIENT_ID=<security-principal-appid>
- export AZURE_SECRET=<security-principal-password>
- export AZURE_TENANT=<security-principal-tenant>

- ansible-playbook-2.7 vmss-create.yaml
```
