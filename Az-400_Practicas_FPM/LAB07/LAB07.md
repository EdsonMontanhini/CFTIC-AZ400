````
az ad sp create-for-rbac --name <service-principal-name>
````

````
az ad sp create-for-rbac --name <service-principal-fpm>
````

````
az account show --query id --output tsv
az account show --query name --output tsv
````



````
{
  "appId": "72892fd3-1150-49ae-ab8f-78efdfbf35ed",
  "displayName": "spdevopsfpm",
  "name": "72892fd3-1150-49ae-ab8f-78efdfbf35ed",
  "password": "6U7~faSyR-iC5RdCoLea.SrY.gSk5s7r.q",
  "tenant": "55fd0f69-040f-4124-8c84-9f878fb4fd13"
}
````

az account show --query id --output tsv

fernando@Azure:~$ az account show --query name --output tsv



**Subscription Id and name**

````
6c41a097-68d1-4fff-ad19-50ab007dcd5c

Azure Pass - Sponsorship
````

**Override Template Parameters**

````
-webAppName $(webappName) -mySQLAdminLoginName "azureuser" -mySQLAdminLoginPassword $(sqldbpassword)
````

**Password Value**

````
P4ssW0rd.fpm.1234
````

