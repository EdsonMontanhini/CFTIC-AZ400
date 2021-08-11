

 # Upload the linked template

PS /home/fnu>  Set-AzureStorageBlobContent `
>>    -Container $containerName `
>>    -File "$home/$fileName" `
>>    -Blob $fileName `
>>    -Context $context

   AccountName: az400m13blob448057162, ContainerName: linktempblobcntr

Name                 BlobType  Length          ContentType                    LastModified         AccessTier SnapshotTime                 IsDeleted  Versio
                                                                                                                                                      nId

----                 --------  ------          -----------                    ------------         ---------- ------------                 ---------  ------

PS /home/fnu> # Generate a SAS token. We set an expiry time of 24 hours, but you could have shorter values for increased security.
PS /home/fnu>  $templateURI = New-AzureStorageBlobSASToken `
>>    -Context $context `
>>    -Container $containerName `
>>    -Blob $fileName `
>>    -Permission r `
>>    -ExpiryTime (Get-Date).AddHours(24.0) `
>>    -FullUri
>>    PS /home/fnu>
>>    PS /home/fnu>  "Resource Group Name: $resourceGroupName"
>>    Resource Group Name: az400m13l01-RG
>>    PS /home/fnu>  "Linked template URI with SAS token: $templateURI"
>>    Linked template URI with SAS token: https://az400m13blob448057162.blob.core.windows.net/linktempblobcntr/storage.json?sv=2019-07-07&sr=b&sig=5FeYaWBmsfN1NXcPSPioa%2B8cfJAUeQsEDgKbmpEldjc%3D&se=2021-08-12T17%3A34%3A29Z&sp=r



URL storage.json

```
https://az400m13blob448057162.blob.core.windows.net/linktempblobcntr/storage.json
```

"diagnosticsProfile": {

​     "bootDiagnostics": {

​      "enabled": true,

​      "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))).primaryEndpoints.blob]"

​     }

​    }

   },



simple-vm

````
20.105.187.36
````

