~ ➜  showcreds
╒═════════════════════════════╤════════════════════════════════════════════════════════════════════╕
│ Name                        │ Value                                                              │
╞═════════════════════════════╪════════════════════════════════════════════════════════════════════╡
│ Azure Console URL           │ https://portal.azure.com/azurefreekmlprod.onmicrosoft.com          │
├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
│ Azure User Name             │ kk_lab_user_main-179af2d41351452f@azurefreekmlprod.onmicrosoft.com │
├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
│ Azure Password              │ h6MMA2L8                                                           │
├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
│ Azure Application Client ID │ f7833cc3-4395-4fb4-bf53-88df282733c2                               │
├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
│ Azure Client Secret         │ bqu8Q~8C6FMxWrxOaCFFbR5PkG1wXt~L63mmXahw                           │
├─────────────────────────────┼────────────────────────────────────────────────────────────────────┤
│ Azure Session End Time      │ Mon May 11 14:45:45 UTC 2026                                       │
╘═════════════════════════════╧════════════════════════════════════════════════════════════════════╛

~ ➜  az login --service-principal \
  --username f7833cc3-4395-4fb4-bf53-88df282733c2 \
  --password 'bqu8Q~8C6FMxWrxOaCFFbR5PkG1wXt~L63mmXahw' \
  --tenant azurefreekmlprod.onmicrosoft.com
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "54c1a2d3-d100-453c-9636-3a109eb45552",
    "id": "f0c3bcdd-5ce2-4fa0-8cf3-41559747512b",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure Free Labs",
    "state": "Enabled",
    "tenantId": "54c1a2d3-d100-453c-9636-3a109eb45552",
    "user": {
      "name": "f7833cc3-4395-4fb4-bf53-88df282733c2",
      "type": "servicePrincipal"
    }
  }
]

~ ➜  az group list -o table
Name                          Location    Status
----------------------------  ----------  ---------
kml_rg_main-179af2d41351452f  eastus      Succeeded

~ ➜  az disk create \
  --name nautilus-disk \
  --resource-group kml_rg_main-179af2d41351452f \
  --size-gb 2 \
  --sku Standard_LRS
{
  "burstingEnabled": null,
  "burstingEnabledTime": null,
  "completionPercent": null,
  "creationData": {
    "createOption": "Empty",
    "elasticSanResourceId": null,
    "galleryImageReference": null,
    "imageReference": null,
    "logicalSectorSize": null,
    "performancePlus": null,
    "securityDataUri": null,
    "sourceResourceId": null,
    "sourceUniqueId": null,
    "sourceUri": null,
    "storageAccountId": null,
    "uploadSizeBytes": null
  },
  "dataAccessAuthMode": null,
  "diskAccessId": null,
  "diskIopsReadOnly": null,
  "diskIopsReadWrite": 500,
  "diskMBpsReadOnly": null,
  "diskMBpsReadWrite": 60,
  "diskSizeBytes": 2147483648,
  "diskSizeGb": 2,
  "diskState": "Unattached",
  "encryption": {
    "diskEncryptionSetId": null,
    "type": "EncryptionAtRestWithPlatformKey"
  },
  "encryptionSettingsCollection": null,
  "extendedLocation": null,
  "hyperVGeneration": null,
  "id": "/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-179af2d41351452f/providers/Microsoft.Compute/disks/nautilus-disk",
  "lastOwnershipUpdateTime": null,
  "location": "eastus",
  "managedBy": null,
  "managedByExtended": null,
  "maxShares": null,
  "name": "nautilus-disk",
  "networkAccessPolicy": "AllowAll",
  "optimizedForFrequentAttach": null,
  "osType": null,
  "propertyUpdatesInProgress": null,
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "purchasePlan": null,
  "resourceGroup": "kml_rg_main-179af2d41351452f",
  "securityProfile": null,
  "shareInfo": null,
  "sku": {
    "name": "Standard_LRS",
    "tier": "Standard"
  },
  "supportedCapabilities": null,
  "supportsHibernation": null,
  "tags": {},
  "tier": null,
  "timeCreated": "2026-05-11T13:47:50.983502+00:00",
  "type": "Microsoft.Compute/disks",
  "uniqueId": "fecef1eb-f8c6-4362-adf5-c031d1f2121b",
  "zones": null
}

~ ➜  az disk show \
  --name nautilus-disk \
  --resource-group kml_rg_main-179af2d41351452f \
  -o table
Name           ResourceGroup                 Location    Zones    Sku           SizeGb    ProvisioningState
-------------  ----------------------------  ----------  -------  ------------  --------  -------------------
nautilus-disk  kml_rg_main-179af2d41351452f  eastus               Standard_LRS  2         Succeeded

~ ➜  
