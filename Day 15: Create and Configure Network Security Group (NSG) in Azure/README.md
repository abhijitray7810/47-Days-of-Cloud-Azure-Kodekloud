````markdown id="a9x2lm"
# Azure Network Security Group (NSG) Setup

This document describes the steps to create a Network Security Group (NSG) for the Nautilus DevOps Azure migration task.

---

# Requirements

Create an NSG with the following configuration:

| Resource | Value |
|---|---|
| NSG Name | `devops-nsg` |
| Rule 1 | Allow HTTP on port `80` |
| Rule 2 | Allow SSH on port `22` |
| Source CIDR | `0.0.0.0/0` |
| Direction | Inbound |

---

# Azure Credentials

```text
Portal URL : https://portal.azure.com
Username   : kk_lab_user_main-272ce395bd1a4523@azurefreekmlprod.onmicrosoft.com
Password   : Xha875JZ
````

---

# Login to Azure

```bash id="k3m7de"
az login
```

---

# Check Available Resource Groups

```bash id="h4q1zc"
az group list --output table
```

Export the resource group and location:

```bash id="u2v5tp"
RG=<resource-group-name>
LOCATION=<location>
```

Example:

```bash id="n8w3yr"
RG=DefaultResourceGroup
LOCATION=eastus
```

---

# Create Network Security Group

```bash id="p7l0sx"
az network nsg create \
  --resource-group $RG \
  --name devops-nsg \
  --location $LOCATION
```

---

# Create HTTP Inbound Rule

Allow HTTP traffic on port `80` from all sources.

```bash id="r5d2jf"
az network nsg rule create \
  --resource-group $RG \
  --nsg-name devops-nsg \
  --name Allow-HTTP \
  --priority 100 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --source-address-prefixes 0.0.0.0/0 \
  --source-port-ranges '*' \
  --destination-port-ranges 80
```

---

# Create SSH Inbound Rule

Allow SSH traffic on port `22` from all sources.

```bash id="m1b8qv"
az network nsg rule create \
  --resource-group $RG \
  --nsg-name devops-nsg \
  --name Allow-SSH \
  --priority 110 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --source-address-prefixes 0.0.0.0/0 \
  --source-port-ranges '*' \
  --destination-port-ranges 22
```

---

# Verify NSG Rules

```bash id="y6t4cn"
az network nsg rule list \
  --resource-group $RG \
  --nsg-name devops-nsg \
  --output table
```

Expected rules:

| Rule Name  | Port | Access |
| ---------- | ---- | ------ |
| Allow-HTTP | 80   | Allow  |
| Allow-SSH  | 22   | Allow  |

---

# Notes

* Ensure Azure CLI is installed on the `azure-client` host.
* Use the provided credentials during login.
* Verify the NSG and rules after creation.

```
```
