````
fnu@Azure:~$ LOCATION=westeurope
fnu@Azure:~$ VERSION=$(az aks get-versions --location $LOCATION --query 'orchestrators[-1].orchestratorVersion' --output tsv); echo $VERSION
1.21.2
fnu@Azure:~$ RGNAME=az400m16l01a-RG
fnu@Azure:~$ az group create --name $RGNAME --location $LOCATION
{
  "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourceGroups/az400m16l01a-RG",
  "location": "westeurope",
  "managedBy": null,
  "name": "az400m16l01a-RG",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}


````



````
fnu@Azure:~$ AKSNAME='az400m16aks'$RANDOM$RANDOM
fnu@Azure:~$ az aks create --location $LOCATION --resource-group $RGNAME --name $AKSNAME --enable-addons monitoring --kubernetes-version $VERSION --generate-ssh-keys
SSH key files '/home/fnu/.ssh/id_rsa' and '/home/fnu/.ssh/id_rsa.pub' have been generated under ~/.ssh to allow SSH access to the VM. If using machines without permanent storage like Azure Cloud Shell without an attached file share, back up your keys to a safe location
Resource provider 'Microsoft.OperationalInsights' used by this operation is not registered. We are registering for you.
Registration succeeded.
Resource provider 'Microsoft.ContainerService' used by this operation is not registered. We are registering for you.
Registration succeeded.
AAD role propagation done[############################################]  100.0000%{
  "aadProfile": null,
  "addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourcegroups/defaultresourcegroup-weu/providers/microsoft.operationalinsights/workspaces/defaultworkspace-236a7cf4-320d-40ca-a14e-a24163bf999d-weu"
      },
      "enabled": true,
      "identity": {
        "clientId": "13ff527a-0250-427f-ace2-3cf3b4aa62db",
        "objectId": "8e3f432a-7c3a-4991-9a7d-d5c395f9f3fe",
        "resourceId": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourcegroups/MC_az400m16l01a-RG_az400m16aks1661916726_westeurope/providers/Microsoft.ManagedIdentity/userAssignedIdentities/omsagent-az400m16aks1661916726"
      }
    }
  },
  "agentPoolProfiles": [
    {
      "availabilityZones": null,
      "count": 3,
      "enableAutoScaling": null,
      "enableEncryptionAtHost": false,
      "enableFips": false,
      "enableNodePublicIp": false,
      "enableUltraSsd": false,
      "gpuInstanceProfile": null,
      "kubeletConfig": null,
      "kubeletDiskType": "OS",
      "linuxOsConfig": null,
      "maxCount": null,
      "maxPods": 110,
      "minCount": null,
      "mode": "System",
      "name": "nodepool1",
      "nodeImageVersion": "AKSUbuntu-1804gen2containerd-2021.08.14",
      "nodeLabels": null,
      "nodePublicIpPrefixId": null,
      "nodeTaints": null,
      "orchestratorVersion": "1.21.2",
      "osDiskSizeGb": 128,
      "osDiskType": "Managed",
      "osSku": "Ubuntu",
      "osType": "Linux",
      "podSubnetId": null,
      "powerState": {
        "code": "Running"
      },
      "provisioningState": "Succeeded",
      "proximityPlacementGroupId": null,
      "scaleSetEvictionPolicy": null,
      "scaleSetPriority": null,
      "spotMaxPrice": null,
      "tags": null,
      "type": "VirtualMachineScaleSets",
      "upgradeSettings": null,
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  "apiServerAccessProfile": null,
  "autoScalerProfile": null,
  "autoUpgradeProfile": null,
  "azurePortalFqdn": "az400m16ak-az400m16l01a-rg-236a7c-40e244dd.portal.hcp.westeurope.azmk8s.io",
  "disableLocalAccounts": null,
  "diskEncryptionSetId": null,
  "dnsPrefix": "az400m16ak-az400m16l01a-RG-236a7c",
  "enablePodSecurityPolicy": null,
  "enableRbac": true,
  "extendedLocation": null,
  "fqdn": "az400m16ak-az400m16l01a-rg-236a7c-40e244dd.hcp.westeurope.azmk8s.io",
  "fqdnSubdomain": null,
  "httpProxyConfig": null,
  "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourcegroups/az400m16l01a-RG/providers/Microsoft.ContainerService/managedClusters/az400m16aks1661916726",
  "identity": {
    "principalId": "22ad149b-e2a1-461b-925e-642545d2fe07",
    "tenantId": "214a4fe3-a171-420c-9b51-8aa9552a3337",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "3deb8fad-21c2-42c3-8481-88f78ae97b84",
      "objectId": "bb4f77d4-3724-41e8-929f-3afcc0e765c1",
      "resourceId": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourcegroups/MC_az400m16l01a-RG_az400m16aks1661916726_westeurope/providers/Microsoft.ManagedIdentity/userAssignedIdentities/az400m16aks1661916726-agentpool"
    }
  },
  "kubernetesVersion": "1.21.2",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDvYXVEDnYb11QXlaeTV4HEHKQw816S5+mlo5vnWnav+E8zOFoT3Krv7CKeSGB9Kdd6gyQ0kMTFWYHii9mra5wvzvSLuNpwqzi8c1soUfdt2X9gUoUl6JxiC/Q+DIsXLhMZaAb7Y1mtNZOr2Q3e08tU/AAlCP/1T0jjNos2CpI3DJTdozq6WLSoYPpyfaZ4j7QAON/TyEqHXoCCzOV0PPgCY9OSSlpFRnXhnrTXHeVa8LR2geuHQ/yj15LDfInf4HoCvrzN2+48/483qcfJcYO4J34YHwfZHUuRSOdAFUEt6lyYLMu9TfL63ybKnzfcCljWvjbYybN8K19nhslhmlmj"
        }
      ]
    }
  },
  "location": "westeurope",
  "maxAgentPools": 100,
  "name": "az400m16aks1661916726",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerProfile": {
      "allocatedOutboundPorts": null,
      "effectiveOutboundIPs": [
        {
          "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourceGroups/MC_az400m16l01a-RG_az400m16aks1661916726_westeurope/providers/Microsoft.Network/publicIPAddresses/47e451ce-e4d7-4cea-911b-654b6038c624",
          "resourceGroup": "MC_az400m16l01a-RG_az400m16aks1661916726_westeurope"
        }
      ],
      "idleTimeoutInMinutes": null,
      "managedOutboundIPs": {
        "count": 1
      },
      "outboundIPs": null,
      "outboundIpPrefixes": null
    },
    "loadBalancerSku": "Standard",
    "networkMode": null,
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "outboundType": "loadBalancer",
    "podCidr": "10.244.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_az400m16l01a-RG_az400m16aks1661916726_westeurope",
  "podIdentityProfile": null,
  "powerState": {
    "code": "Running"
  },
  "privateFqdn": null,
  "privateLinkResources": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "az400m16l01a-RG",
  "servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  },
  "sku": {
    "name": "Basic",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters",
  "windowsProfile": null
}
````

````
SQLNAME='az400m16sql'$RANDOM$RANDOM
az sql server create --location $LOCATION --resource-group $RGNAME --name $SQLNAME --admin-user sqladmin --admin-password P2ssw0rd1234
````

````
fnu@Azure:~$ SQLNAME='az400m16sql'$RANDOM$RANDOM
fnu@Azure:~$ az sql server create --location $LOCATION --resource-group $RGNAME --name $SQLNAME --admin-user sqladmin --admin-password P2ssw0rd1234
None
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "administrators": null,
  "federatedClientId": null,
  "fullyQualifiedDomainName": "az400m16sql803916298.database.windows.net",
  "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourceGroups/az400m16l01a-RG/providers/Microsoft.Sql/servers/az400m16sql803916298",
  "identity": null,
  "keyId": null,
  "kind": "v12.0",
  "location": "westeurope",
  "minimalTlsVersion": null,
  "name": "az400m16sql803916298",
  "primaryUserAssignedIdentityId": null,
  "privateEndpointConnections": [],
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "az400m16l01a-RG",
  "restrictOutboundNetworkAccess": "Disabled",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0",
  "workspaceFeature": null
}
````

````
fnu@Azure:~$ az sql server firewall-rule create --resource-group $RGNAME --server $SQLNAME --name allowAzure --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
{
  "endIpAddress": "0.0.0.0",
  "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourceGroups/az400m16l01a-RG/providers/Microsoft.Sql/servers/az400m16sql803916298/firewallRules/allowAzure",
  "name": "allowAzure",
  "resourceGroup": "az400m16l01a-RG",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.Sql/servers/firewallRules"
}
````

````
fnu@Azure:~$ az sql server firewall-rule create --resource-group $RGNAME --server $SQLNAME --name allowAzure --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
{
  "endIpAddress": "0.0.0.0",
  "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourceGroups/az400m16l01a-RG/providers/Microsoft.Sql/servers/az400m16sql803916298/firewallRules/allowAzure",
  "name": "allowAzure",
  "resourceGroup": "az400m16l01a-RG",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.Sql/servers/firewallRules"
}
fnu@Azure:~$ az sql db create --resource-group $RGNAME --server $SQLNAME --name mhcdb --service-objective S0 --no-wait
fnu@Azure:~$
````

````
fnu@Azure:~$ ACRNAME='az400m16acr'$RANDOM$RANDOM
fnu@Azure:~$ az acr create --location $LOCATION --resource-group $RGNAME --name $ACRNAME --sku Standard
{
  "adminUserEnabled": false,
  "anonymousPullEnabled": false,
  "creationDate": "2021-09-01T06:47:13.672923+00:00",
  "dataEndpointEnabled": false,
  "dataEndpointHostNames": [],
  "encryption": {
    "keyVaultProperties": null,
    "status": "disabled"
  },
  "id": "/subscriptions/236a7cf4-320d-40ca-a14e-a24163bf999d/resourceGroups/az400m16l01a-RG/providers/Microsoft.ContainerRegistry/registries/az400m16acr215925049",
  "identity": null,
  "location": "westeurope",
  "loginServer": "az400m16acr215925049.azurecr.io",
  "name": "az400m16acr215925049",
  "networkRuleBypassOptions": "AzureServices",
  "networkRuleSet": null,
  "policies": {
    "quarantinePolicy": {
      "status": "disabled"
    },
    "retentionPolicy": {
      "days": 7,
      "lastUpdatedTime": "2021-09-01T06:47:16.430485+00:00",
      "status": "disabled"
    },
    "trustPolicy": {
      "status": "disabled",
      "type": "Notary"
    }
  },
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "az400m16l01a-RG",
  "sku": {
    "name": "Standard",
    "tier": "Standard"
  },
  "status": null,
  "systemData": {
    "createdAt": "2021-09-01T06:47:13.672923+00:00",
    "createdBy": "fernanipmotech@fernanipmotech.onmicrosoft.com",
    "createdByType": "User",
    "lastModifiedAt": "2021-09-01T06:47:13.672923+00:00",
    "lastModifiedBy": "fernanipmotech@fernanipmotech.onmicrosoft.com",
    "lastModifiedByType": "User"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries",
  "zoneRedundancy": "Disabled"
}
````

````
# Retrieve the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $RGNAME --name $AKSNAME --query "identityProfile.kubeletidentity.clientId" --output tsv)

# Retrieve the ACR registry resource id
ACR_ID=$(az acr show --name $ACRNAME --resource-group $RGNAME --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID

# Retrieve the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $RGNAME --name $AKSNAME --query "identityProfile.kubeletidentity.clientId" --output tsv)

# Retrieve the ACR registry resource id
ACR_ID=$(az acr show --name $ACRNAME --resource-group $RGNAME --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
````

