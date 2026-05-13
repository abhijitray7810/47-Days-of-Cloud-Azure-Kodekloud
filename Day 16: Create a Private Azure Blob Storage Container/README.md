````markdown
# Azure Blob Storage Setup

This document outlines the steps performed to create an Azure Storage Account and a private Blob container for the Nautilus DevOps data migration project.

---

## Resources Created

### Storage Account
- **Name:** `devopsst13136`
- **Type:** `StorageV2`
- **SKU:** `Standard_LRS`
- **Location:** `eastus`

### Blob Container
- **Name:** `devops-blob-8307`
- **Access Level:** `Private`

---

## Resource Group

- **Name:** `kml_rg_main-c59871a2b32e4f86`

---

## Azure CLI Commands Used

### 1. Login to Azure

```bash
az login
````

---

### 2. Verify Resource Group

```bash
az group list --output table
```

---

### 3. Create Storage Account

```bash
az storage account create \
  --name devopsst13136 \
  --resource-group kml_rg_main-c59871a2b32e4f86 \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2
```

---

### 4. Retrieve Storage Account Key

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --resource-group kml_rg_main-c59871a2b32e4f86 \
  --account-name devopsst13136 \
  --query '[0].value' \
  --output tsv)
```

---

### 5. Create Private Blob Container

```bash
az storage container create \
  --name devops-blob-8307 \
  --account-name devopsst13136 \
  --account-key $ACCOUNT_KEY \
  --public-access off
```

---

### 6. Verify Blob Container

```bash
az storage container list \
  --account-name devopsst13136 \
  --account-key $ACCOUNT_KEY \
  --output table
```

---

## Verification Output

```text
Name              Lease Status    Last Modified
----------------  --------------  -------------------------
devops-blob-8307                  2026-05-13T08:10:55+00:00
```

---

## Status

✅ Storage account and private Blob container created successfully.

```
```
