````bash
LOCATION='westeurope'
RG_NAME='az400m1501a-RG'
ACR_NAME=az400m151acr$RANDOM$RANDOM APP_SVC_PLAN='az400m1501a-app-svc-plan' WEB_APP_NAME=az400m151web$RANDOM$RANDOM
SQLDB_SRV_NAME=az400m15sqlsrv$RANDOM$RANDOM
SQLDB_NAME='az400m15sqldb'
az group create --name $RG_NAME --location $LOCATION 
az acr create --name $ACR_NAME --resource-group $RG_NAME --location $LOCATION --sku Standard --admin-enabled true
az appservice plan create --name 'az400m1501a-app-svc-plan' --location $LOCATION --resource-group $RG_NAME --is-linux
az webapp create --name $WEB_APP_NAME --resource-group $RG_NAME --plan $APP_SVC_PLAN --deployment-container-image-name elnably/dockerimagetest
IMAGE_NAME=myhealth.web 
az webapp config container set --name $WEB_APP_NAME --resource-group $RG_NAME --docker-custom-image-name $IMAGE_NAME --docker-registry-server-url $ACR_NAME.azurecr.io/$IMAGE_NAME:latest --docker-registry-server-url https://$ACR_NAME.azurecr.io
az sql server create --name $SQLDB_SRV_NAME --resource-group $RG_NAME --location $LOCATION --admin-user sqladmin --admin-password Pa55w.rd1234
az sql db create --name $SQLDB_NAME --resource-group $RG_NAME --server $SQLDB_SRV_NAME --service-objective S0 --no-wait
az sql server firewall-rule create --name AllowAllAzure --resource-group $RG_NAME --server $SQLDB_SRV_NAME --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
````



````bash
CONNECTION_STRING="DataSource=tcp:az400m15sqlsrv65173345.database.windows.net,1433;Initial Catalog=az400m15sqldb;User Id=sqladmin;Password=Pa55w.rd1234;"
az webapp config connection-string set --name az400m151web608630705 --resource-group az400m1501a-RG --connection-string-type SQLAzure --settings defaultConnection="$CONNECTION_STRING"
````

fnu@Azure:~$ CONNECTION_STRING="DataSource=tcp:az400m15sqlsrv65173345.database.windows.net,1433;Initial Catalog=az400m15sqldb;User Id=sqladmin;Password=Pa55w.rd1234;"
fnu@Azure:~$ az webapp config connection-string set --name az400m151web608630705 --resource-group az400m1501a-RG --connection-string-type SQLAzure --settings defaultConnection="$CONNECTION_STRING"
{
  "defaultConnection": {
    "type": "SQLAzure",
    "value": "DataSource=tcp:az400m15sqlsrv65173345.database.windows.net,1433;Initial Catalog=az400m15sqldb;User Id=sqladmin;Password=Pa55w.rd1234;"
  }
}
fnu@Azure:~$



Registry name: az400m151acr629530906

Login server: az400m151acr629530906.azurecr.io

User name: az400m151acr629530906

Password: QNZjkBg2jscybJmK0de25hAe+Pxri8n3

Password2: Ak=1sSax26160RJEpi7Eyk4xE8SIGjVG