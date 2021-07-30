{
  "fqdns": "",
  "id": "/subscriptions/3c13580b-f1fd-4fd4-9c0e-b29f2197e393/resourceGroups/az400m14l03rg/providers/Microsoft.Compute/virtualMachines/az400m1403vm1",
  "location": "westeurope",
  "macAddress": "00-0D-3A-4B-E6-E4",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "20.103.255.72",
  "resourceGroup": "az400m14l03rg",
  "zones": ""
}

PIP=$(az vm show --show-details --resource-group $RG1NAME --name $VM1NAME --query publicIps --output tsv) ssh azureuser@$20.103.255.72

azureuser@az400m1403vm1:~$ az login
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code DCMFQCJ7X to authenticate.

| Setting         | Value                 |
| :-------------- | :-------------------- |
| Resource group  | **az400m14l03arg**    |
| Virtual network | **az400m1403aVNET**   |
| Subnet          | **az400m1403aSubnet** |

admin_username: azureuser

**Public Key Linux VM (ssh-rsa)**

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCToBSA796mf1ckWqElb259dx6TdR99gW93Xsj/uJdQV1Rrr/xrUIKBH9aw/0sCk32UCAEBS+/4ZG8CFWfbEveZ/JX/iFy3txPLMsb9hNZtSPWl7ggjb11/3EpPMWmIKgFgn1sNxHWVMS8EPyuR8OLXkO+D0847mWMp45DrR+J6qYaq1mzxFm+vjlDBDqXenwlzfZKx02prB7VvtWGYGSvdRoAqYrG1/KudNH6Ha5NfH8IyDJlxHeSIx4X9gUp+wY7iOrmgVA25cEp4R/DmHIM8Pvkr2XQQJlOBZ/pXN75xfekC2ZUuJ81AgMjHKBXua0pMtyWUvyGWMM7FnMhSpu4D azureuser@az400m1403vm1

```

**Sample Ansible playbook**

````
sudo ansible-playbook ~/PartsUnlimitedMRP/Labfiles/AZ-400T05-ImplemntgAppInfra/Labfiles/ansible/new_vm_web.yml --extra-vars "vmname=vmfpm202107 resgrp=az400m14l03arg vnet=az400m1403aVNET subnet=az400m1403aSubnet"
````

azureuser@az400m1403vm1:~$ sudo ansible --user azureuser --private-key=/home/azureuser/.ssh/id_rsa all -m ping -i ./myazure_rm.yml
[DEPRECATION WARNING]: Ansible will require Python 3.8 or newer on the controller starting with Ansible 2.12. Current version: 3.6.9 (default, Jan 26 2021,
15:33:00) [GCC 8.4.0]. This feature will be removed from ansible-core in version 2.12. Deprecation warnings can be disabled by setting
deprecation_warnings=False in ansible.cfg.
vmfpm202107_e88c | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}



**Configure a VM using Ansible playbook**

````
RGNAME='az400m14l03arg'
VMNAME='vmfpm202107'
PIP=$(az vm show --show-details --resource-group $RGNAME --name $VMNAME --query publicIps --output tsv)
````

**Installing the http service**

````
sudo ansible-playbook --user azureuser --private-key=/home/azureuser/.ssh/id_rsa -i ./myazure_rm.yml ~/PartsUnlimitedMRP/Labfiles/AZ-400T05-ImplemntgAppInfra/Labfiles/ansible/httpd.yml --extra-vars "vmname=vmfpm202107*"
````

azureuser@az400m1403vm1:~$ curl http://$PIP
curl: (7) Failed to connect to 52.178.117.176 port 80: Connection refused



azureuser@az400m1403vm1:~$ curl http://$PIP
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello World</h1>
        <p>
            <br>This is a test page
            <br>This is a test page
            <br>This is a test page
        </p>
    </body>

