Update the file so it looks like this:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "functions": [],
    "variables": {},
    "resources": [
        {
            "name": "arm-vnet-datacenter",
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2023-11-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "arm-vnet-datacenter",
                "Environment": "KKE-datacenter"
            },
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "192.168.0.0/16"
                    ]
                }
            }
        }
    ],
    "outputs": {
    }
}
```

Then run:

```bash
az group list --query '[].name' --output table | grep 'kml'
```

Copy the resource group name from the output and deploy:

```bash
az deployment group create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --template-file vnet-deployment-template.json
```

Example:

```bash
az deployment group create \
  --resource-group kml-rg \
  --template-file vnet-deployment-template.json
```
