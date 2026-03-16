# Azure Virtual Machine Creation using Azure CLI

## Overview

This project demonstrates how to create an Azure Virtual Machine (VM) using the Azure CLI from a client host. The VM is created with specific configurations such as Ubuntu 22.04 image, Standard_B2s size, and secure SSH authentication.

---

## Prerequisites

* Access to a system with **Azure CLI installed**
* Logged into an Azure account
* Permissions to create resources within the assigned Resource Group

---

## Azure Account Verification

Verify the active Azure account:

```bash
az account show
```

List available resource groups:

```bash
az group list
```

Identify the existing resource group to use.

Example:

```bash
RG_NAME="kml_rg_main-8c80fd6043ca4914"
```

---

## Create the Virtual Machine

Run the following command to create the VM:

```bash
az vm create \
  --resource-group $RG_NAME \
  --name nautilus-vm \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30
```

---

## Configuration Details

| Parameter      | Value        |
| -------------- | ------------ |
| VM Name        | nautilus-vm  |
| Image          | Ubuntu 22.04 |
| VM Size        | Standard_B2s |
| Admin Username | azureuser    |
| Authentication | SSH Key      |
| Storage Type   | Standard_LRS |
| Disk Size      | 30 GB        |

---

## Verify VM Status

Confirm the VM is running:

```bash
az vm show \
  --name nautilus-vm \
  --resource-group $RG_NAME \
  --show-details \
  --query powerState \
  -o tsv
```

Expected output:

```
VM running
```

---

## Result

The Azure Virtual Machine **nautilus-vm** is successfully created with the required configuration and is in a running state.

---

## Author

DevOps Lab Implementation
