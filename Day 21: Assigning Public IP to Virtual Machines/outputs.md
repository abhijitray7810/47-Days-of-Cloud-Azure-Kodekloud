
~ ➜  az group list --query '[].name' --output table | grep 'kml'
kml_rg_main-ff68086ee9e0438b

~ ➜  RG_NAME=kml_rg_main-ff68086ee9e0438b

~ ➜  ssh-keygen -t rsa -b 2048 -f ~/.ssh/xfusion_key -N ""
Generating public/private rsa key pair.
Your identification has been saved in /root/.ssh/xfusion_key
Your public key has been saved in /root/.ssh/xfusion_key.pub
The key fingerprint is:
SHA256:oj9nS6jcMg3lHdYv79LB66YsI8ThKJEUj1ZwN3gwxlE root@azure-client
The key's randomart image is:
+---[RSA 2048]----+
|  o+B=E          |
|  .*oo..         |
| .o...   .       |
| .o   o o .      |
|   . *.+S. o     |
|  . o.=o. . +    |
|   ..+. .  + o   |
|   .o++.=.. =    |
|    ooo=.+o*o    |
+----[SHA256]-----+

~ ➜  az network public-ip create \
  --resource-group $RG_NAME \
  --name xfusion-pip \
  --sku Standard \
  --allocation-method Static \
  --location centralus
[Coming breaking change] In the coming release, the default behavior will be changed as follows when sku is Standard and zone is not provided: For zonal regions, you will get a zone-redundant IP indicated by zones:["1","2","3"]; For non-zonal regions, you will get a non zone-redundant IP indicated by zones:null.
{
  "publicIp": {
    "ddosSettings": {
      "protectionMode": "VirtualNetworkInherited"
    },
    "etag": "W/\"61cc3d74-33d5-44c3-8cfe-6482a26d4ee9\"",
    "id": "/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Network/publicIPAddresses/xfusion-pip",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "20.12.221.96",
    "ipTags": [],
    "location": "centralus",
    "name": "xfusion-pip",
    "provisioningState": "Succeeded",
    "publicIPAddressVersion": "IPv4",
    "publicIPAllocationMethod": "Static",
    "resourceGroup": "kml_rg_main-ff68086ee9e0438b",
    "resourceGuid": "53baa41d-b77b-4a99-b76f-3fd1bf480430",
    "sku": {
      "name": "Standard",
      "tier": "Regional"
    },
    "type": "Microsoft.Network/publicIPAddresses"
  }
}

~ ➜  az vm create \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/xfusion_key.pub \
  --public-ip-address xfusion-pip \
  --location centralus
{"status":"Failed","error":{"code":"DeploymentFailed","target":"/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Resources/deployments/vm_deploy_lCRnZ50NwywzRl7NvQEqQHJrMCjopW87","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-deployment-operations for usage details.","details":[{"code":"ResourceDeploymentFailure","target":"/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Compute/virtualMachines/xfusion-vm","message":"The resource write operation failed to complete successfully, because it reached terminal provisioning state 'Failed'.","details":[{"code":"RequestDisallowedByPolicy","message":"Resource 'xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6' was disallowed by policy. Reasons: 'Global: This storage configuration is not allowed. Ensure the disk size is 128 GB or less and the SKU is not Premium for Compute disks. For Storage accounts, use Standard_LRS or Standard_RAGRS SKUs.'. See error details for policy resource IDs.  Target: '/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Compute/disks/xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6'."}]}]}}

~ ✖ az vm create \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/xfusion_key.pub \
  --public-ip-address xfusion-pip \
  --location centralus \
  --storage-sku Standard_LRS
{"status":"Failed","error":{"code":"DeploymentFailed","target":"/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Resources/deployments/vm_deploy_KtAY9x7reM5xapsjObCrfAJe2rZ99Nqs","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-deployment-operations for usage details.","details":[{"code":"OperationNotAllowed","target":"osDisk.managedDisk.storageAccountType","message":"Managed disk storage account type change through Virtual Machine 'xfusion-vm' is not allowed. Please update disk resource at /subscriptions/723f5558-8ae1-4d65-aedb-e4a48a9b7ea2/resourceGroups/31db7aab-c26c-4f8b-9db3-d61d7772e339/providers/Microsoft.Compute/disks/pps-vm-01_pps-vm-01_0_OsDisk_1_2979baff914a464895575f173e79561e."}]}}

~ ✖ az vm delete \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --yes
(RequestDisallowedByPolicy) Resource 'xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6' was disallowed by policy. Reasons: 'Global: This storage configuration is not allowed. Ensure the disk size is 128 GB or less and the SKU is not Premium for Compute disks. For Storage accounts, use Standard_LRS or Standard_RAGRS SKUs.'. See error details for policy resource IDs.  Target: '/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/KML_RG_MAIN-FF68086EE9E0438B/providers/Microsoft.Compute/disks/xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6'.
Code: RequestDisallowedByPolicy
Message: Resource 'xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6' was disallowed by policy. Reasons: 'Global: This storage configuration is not allowed. Ensure the disk size is 128 GB or less and the SKU is not Premium for Compute disks. For Storage accounts, use Standard_LRS or Standard_RAGRS SKUs.'. See error details for policy resource IDs.  Target: '/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/KML_RG_MAIN-FF68086EE9E0438B/providers/Microsoft.Compute/disks/xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6'.

~ ✖ az disk list \
  --resource-group $RG_NAME \
  --output table


~ ➜  az disk delete \
  --resource-group $RG_NAME \
  --name xfusion-vm_disk1_0e674b7e77cd4319a24a43d23eda21d6 \
  --yes

~ ➜  az resource delete \
  --resource-group $RG_NAME \
  --resource-type Microsoft.Compute/virtualMachines \
  --name xfusion-vm
Some resources failed to be deleted (run with `--verbose` for more information):
/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b

~ ✖ az vm create \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/xfusion_key.pub \
  --public-ip-address xfusion-pip \
  --location centralus \
  --storage-sku Standard_LRS
{"status":"Failed","error":{"code":"DeploymentFailed","target":"/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Resources/deployments/vm_deploy_pHJVLqao3C7pILkVlechsjaLiz8PP1ZE","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-deployment-operations for usage details.","details":[{"code":"OperationNotAllowed","message":"Operation 'Update VM' is not allowed on VM 'xfusion-vm' since the VM is marked for deletion. You can only retry the Delete operation (or wait for an ongoing one to complete)."}]}}

~ ✖ az vm show \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  -d \
  --query publicIps \
  --output tsv
20.12.221.96

~ ➜  az resource delete \
  --ids /subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/$RG_NAME/providers/Microsoft.Compute/virtualMachines/xfusion-vm
Some resources failed to be deleted (run with `--verbose` for more information):
/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Compute/virtualMachines/xfusion-vm

~ ✖ az vm show \
  --resource-group $RG_NAME \
  --name xfusion-vm
{
  "additionalCapabilities": null,
  "applicationProfile": null,
  "availabilitySet": null,
  "billingProfile": null,
  "capacityReservation": null,
  "diagnosticsProfile": null,
  "etag": "\"2\"",
  "evictionPolicy": null,
  "extendedLocation": null,
  "extensionsTimeBudget": null,
  "hardwareProfile": {
    "vmSize": "Standard_B1s",
    "vmSizeProperties": null
  },
  "host": null,
  "hostGroup": null,
  "id": "/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Compute/virtualMachines/xfusion-vm",
  "identity": null,
  "instanceView": null,
  "licenseType": null,
  "location": "centralus",
  "managedBy": null,
  "name": "xfusion-vm",
  "networkProfile": {
    "networkApiVersion": null,
    "networkInterfaceConfigurations": null,
    "networkInterfaces": [
      {
        "deleteOption": null,
        "id": "/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Network/networkInterfaces/xfusion-vmVMNic",
        "primary": null,
        "resourceGroup": "kml_rg_main-ff68086ee9e0438b"
      }
    ]
  },
  "osProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "allowExtensionOperations": true,
    "computerName": "xfusion-vm",
    "customData": null,
    "linuxConfiguration": {
      "disablePasswordAuthentication": true,
      "enableVmAgentPlatformUpdates": null,
      "patchSettings": {
        "assessmentMode": "ImageDefault",
        "automaticByPlatformSettings": null,
        "patchMode": "ImageDefault"
      },
      "provisionVmAgent": true,
      "ssh": {
        "publicKeys": [
          {
            "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDlQLHaennKQ7qTWSjeJ2U6KgsHvxpwBkkdeiH/DSWRHpf6Gi35vYVGA8znY52AmRvuzMIKFKQMTH7lsT+/w68HrbBW3wNdda1NTSo1i+0zj6PtYxFdJCbMkND9Ljr4+XR3KIn47n9YtoBLBSJxSdekij5WOeZJ85kYiymdfXZfr0I5x7QJV+OlGSwAV3RtWLhGEzb2x2BbrpbBOWoS/tesV/e+QR81tM27MFlkco7LILX+75kTnPje0gQRAPJBhIcgilk9ewmJKDr8SLJurMn3S78CzbAkHLEUqpHG1s7StecT7oTU/1171C/dpXJO8Jg+OjhyH9+P+i4FY3QskHwN root@azure-client\n",
            "path": "/home/azureuser/.ssh/authorized_keys"
          }
        ]
      }
    },
    "requireGuestProvisionSignal": true,
    "secrets": [],
    "windowsConfiguration": null
  },
  "plan": null,
  "platformFaultDomain": null,
  "priority": null,
  "provisioningState": "Failed",
  "proximityPlacementGroup": null,
  "resourceGroup": "kml_rg_main-ff68086ee9e0438b",
  "resources": null,
  "scheduledEventsPolicy": null,
  "scheduledEventsProfile": null,
  "securityProfile": {
    "encryptionAtHost": null,
    "encryptionIdentity": null,
    "proxyAgentSettings": null,
    "securityType": "TrustedLaunch",
    "uefiSettings": {
      "secureBootEnabled": true,
      "vTpmEnabled": true
    }
  },
  "storageProfile": {
    "dataDisks": [],
    "diskControllerType": "SCSI",
    "imageReference": {
      "communityGalleryImageId": null,
      "exactVersion": "22.04.202605030",
      "id": null,
      "offer": "0001-com-ubuntu-server-jammy",
      "publisher": "Canonical",
      "sharedGalleryImageId": null,
      "sku": "22_04-lts-gen2",
      "version": "latest"
    },
    "osDisk": {
      "caching": "ReadWrite",
      "createOption": "FromImage",
      "deleteOption": "Detach",
      "diffDiskSettings": null,
      "diskSizeGb": 30,
      "encryptionSettings": null,
      "image": null,
      "managedDisk": {
        "diskEncryptionSet": null,
        "id": null,
        "securityProfile": null,
        "storageAccountType": "Premium_LRS"
      },
      "name": null,
      "osType": "Linux",
      "vhd": null,
      "writeAcceleratorEnabled": null
    }
  },
  "tags": {},
  "timeCreated": "2026-05-24T05:40:19.989489+00:00",
  "type": "Microsoft.Compute/virtualMachines",
  "userData": null,
  "virtualMachineScaleSet": null,
  "vmId": "4a213402-713f-4d85-b908-4e4f5897554c",
  "zones": null
}

~ ➜  az vm delete \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --yes \
  --force-deletion yes

~ ➜  az vm show \
  --resource-group $RG_NAME \
  --name xfusion-vm
(ResourceNotFound) The Resource 'Microsoft.Compute/virtualMachines/xfusion-vm' under resource group 'kml_rg_main-ff68086ee9e0438b' was not found. For more details please go to https://aka.ms/ARMResourceNotFoundFix
Code: ResourceNotFound
Message: The Resource 'Microsoft.Compute/virtualMachines/xfusion-vm' under resource group 'kml_rg_main-ff68086ee9e0438b' was not found. For more details please go to https://aka.ms/ARMResourceNotFoundFix

~ ✖ az vm create \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/xfusion_key.pub \
  --public-ip-address xfusion-pip \
  --location centralus \
  --storage-sku StandardSSD_LRS \
  --os-disk-size-gb 30
{
  "fqdns": "",
  "id": "/subscriptions/f0c3bcdd-5ce2-4fa0-8cf3-41559747512b/resourceGroups/kml_rg_main-ff68086ee9e0438b/providers/Microsoft.Compute/virtualMachines/xfusion-vm",
  "location": "centralus",
  "macAddress": "00-0D-3A-A7-3A-21",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "20.12.221.96",
  "resourceGroup": "kml_rg_main-ff68086ee9e0438b",
  "zones": ""
}

~ ➜  az vm show \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  -d \
  --query "{VM:name,IP:publicIps,State:powerState}" \
  --output table
VM          IP            State
----------  ------------  ----------
xfusion-vm  20.12.221.96  VM running

~ ➜  
