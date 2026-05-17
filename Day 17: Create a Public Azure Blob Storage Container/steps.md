Use the existing resource group again.

First get the resource group name:

```bash id="w5f2xq"
az group list --output table
```

Set it:

```bash id="v8m1pa"
RG_NAME="kml_rg_main-d9eea92ff24a487f"
```

Create the storage account with public blob access enabled:

```bash id="q4z7bt"
az storage account create \
  --name datacenterst13796 \
  --resource-group $RG_NAME \
  --location eastus \
  --sku Standard_LRS \
  --allow-blob-public-access true
```

Get the account key:

```bash id="y6n2lc"
ACCOUNT_KEY=$(az storage account keys list \
  --resource-group $RG_NAME \
  --account-name datacenterst13796 \
  --query '[0].value' \
  -o tsv)
```

Create the container with full public anonymous access:

```bash id="g3u9kr"
az storage container create \
  --name datacenter-blob-31641 \
  --account-name datacenterst13796 \
  --account-key $ACCOUNT_KEY \
  --public-access container
```

Verify:

```bash id="k8r1pw"
az storage container show \
  --name datacenter-blob-31641 \
  --account-name datacenterst13796 \
  --account-key $ACCOUNT_KEY \
  --query properties.publicAccess
```

Expected output:

```text id="c1t5mv"
"container"
```
