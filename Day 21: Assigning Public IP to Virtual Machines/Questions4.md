
The Nautilus DevOps Team has received a new request from the Development Team to set up a new Azure Virtual Machine (VM). This VM will be used to host a new application that requires a stable public IP address. To ensure that the VM has a consistent public IP, a Static Public IP address needs to be associated with it. The VM will be named devops-vm, and the Static Public IP will be named devops-pip. This setup will help the Development Team to have a reliable and consistent access point for their application.

Create an Azure VM named devops-vm using any available Ubuntu image, with the VM size Standard_B1s.
Generate an SSH public key on the azure-client host and associate it with the VM for SSH access.
Associate a Static Public IP address named devops-pip with this VM.
Ensure the VM is accessible via SSH using the generated public key.

Use below given Azure Credentials: (You can run the showcreds command on the azure-client host to retrieve these credentials)

Portal URL	https://portal.azure.com
Username	kk_lab_user_main-2512ef8eb8cd4d73@azurefreekmlprod.onmicrosoft.com
Password	T3-7UV6D
Start Time	Thu Jun 11 12:31:10 UTC 2026
End Time	Thu Jun 11 13:31:10 UTC 2026

Notes:

Perform all operations in the Central US region.
