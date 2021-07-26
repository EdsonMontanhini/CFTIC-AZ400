**List resource-groups:**

````
az group list --query "[?starts_with(name,'az400m08l01')].name" --output tsv
````

````
az group list --query "[?starts_with(name,'VstsRG-az400m08l01')].name" --output tsv
````

````
az group list --query "[?starts_with(name,'webapp08fpm')].name" --output tsv
````

**Remove resource-groups**

````
az group list --query "[?starts_with(name,'az400m08l01')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
````

````
az group list --query "[?starts_with(name,'VstsRG-az400m08l01')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
````

````
az group list --query "[?starts_with(name,'webapp08fpm')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
````



**Resource-groups**

````
VstsRG-az400m08l01-6f9e
````

````
webapp08fpm-rg
````





