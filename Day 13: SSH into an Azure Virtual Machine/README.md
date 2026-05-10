The password has a special character that needs to be escaped properly. Let me fix that:The sandbox I'm running in blocks Azure domains. **You need to run this on your actual Azure client host (landing host).** Here are the corrected commands — note the password must be in **single quotes** to prevent the shell from interpreting `=`:

---

### Step 1 — Login (single-quoted password)
```bash
az login -u "kk_lab_user_main-ec918e284ed1432e@azurefreekmlprod.onmicrosoft.com" \
         -p '7=xkk9PW' \
         --tenant "azurefreekmlprod.onmicrosoft.com"
```

### Step 2 — Get VM public IP
```bash
RG=$(az vm list --query "[?name=='xfusion-vm'].resourceGroup" -o tsv)
VM_IP=$(az vm show -d -g "$RG" -n xfusion-vm --query publicIps -o tsv)
echo "VM IP: $VM_IP"
```

### Step 3 — Generate root SSH key if missing
```bash
mkdir -p /root/.ssh && chmod 700 /root/.ssh
[ -f /root/.ssh/id_rsa ] || ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N ""
```

### Step 4 — SSH into the VM as azureuser (accept host key), then add root's public key
```bash
ssh -o StrictHostKeyChecking=no azureuser@$VM_IP "
  sudo mkdir -p /root/.ssh
  sudo chmod 700 /root/.ssh
  echo '$(cat /root/.ssh/id_rsa.pub)' | sudo tee -a /root/.ssh/authorized_keys
  sudo chmod 600 /root/.ssh/authorized_keys
  sudo chown -R root:root /root/.ssh
"
```

### Step 5 — Enable PermitRootLogin and restart SSH
```bash
ssh azureuser@$VM_IP "
  sudo sed -i 's/^#*PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
  sudo systemctl restart sshd
"
```

### Step 6 — Verify passwordless root SSH
```bash
ssh -o StrictHostKeyChecking=no root@$VM_IP "whoami && echo 'Root SSH: SUCCESS'"
```

---

Run these **on your Azure client/landing host terminal** (not here). The key fix is wrapping the password in **single quotes** (`'7=xkk9PW'`) so the `=` isn't misinterpreted by the shell. Paste the output of each step here if you hit any errors and I'll help debug.
