# jenkinsslave-ansible![ansi](https://user-images.githubusercontent.com/107124664/188657563-b1f1335a-506c-41d6-ac5e-c04828b9b5de.PNG)


![jenkins](https://user-images.githubusercontent.com/107124664/188887474-77938bf4-d455-4a64-b628-7510f7cc537d.png)

Problem statement: Need to deploy windows core machine with jenkins slave running in it.

prerequisites:
1. A running jenkins master server in the same server the ansible controller - Installed & configured by creating azure credentials file. 
pip install "pywinrm>=0.3.0"
2.Install Azure dependencies package
pip install ansible[azure]
3. Azure service principal: Create a service principal, making note of the following values: appId, displayName, password, and tenant (azurecli)

curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
ansible-galaxy init /etc/ansible/roles/configure --offline

steps:
1. inorder to do jenkins slave setup we require an windows core vm so for provisioning VM - here we used ansible automation for the same

*Login to azure with azure cli az login
*Select the correct subscription with the command az account set --subscription "sub name"`
*Launch with ansible-playbook azure.yml

2. once VM deployed place the slave ip in hosts.ini for configuring jenkins slave setup 

run playbook ansible-playbook --inventory=hosts.ini site.yml

if the playbook triggers & got winrm connection error we have to follow below commands in windows VM 

$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file



