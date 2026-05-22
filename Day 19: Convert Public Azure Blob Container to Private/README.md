
Your Azure CLI version only supports key-based auth for this command. Use the account key instead.

Run these commands:

```bash id="c2n8ya"
az login
```

Complete the browser/device login using the provided Azure credentials.

Then get the storage account key:

```bash id="m5q7rw"
ACCOUNT_KEY=$(az storage account keys list \
  --account-name nautilusst18458 \
  --query '[0].value' -o tsv)
```

Now change the container from public to private:

```bash id="p4v6sx"
az storage container set-permission \
  --account-name nautilusst18458 \
  --account-key "$ACCOUNT_KEY" \
  --name nautilus-container-19168 \
  --public-access off
```

Verify:

```bash id="t8u1kn"
az storage container show \
  --account-name nautilusst18458 \
  --account-key "$ACCOUNT_KEY" \
  --name nautilus-container-19168 \
  --query properties.publicAccess

az storage container show \
  --account-name nautilusst18458 \
  --account-key "$ACCOUNT_KEY" \
  --name nautilus-priv-20623 \
  --query properties.publicAccess
```

Expected output for both:

```text id="x7f2ld"
null
```

`null` confirms the containers are private with no public access.
