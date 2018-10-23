This demo will
1) install Windows AD Server on a windows 2012 or later Server 
2) Setup OpenSSH Service on the Windows Server
3) Create users on the Windows AD on that Server


Pre-Requisites
1) You will need access to a Windows 2012 Server to run this demo.
opentlc automation->ansible tower lab provisions one
2) Configure WinRM on the server:
Can be configured by following powershell script:
"<powershell>\n",
"$admin = [adsi]('WinNT://./administrator, user')\n",
"$admin.PSBase.Invoke('SetPassword', '{{ windows_password | default(generated_windows_password) }}')\n",
"$scriptPath=((New-Object System.Net.Webclient).DownloadString('https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1'))\n",
"Invoke-Command -ScriptBlock ([scriptblock]::Create($scriptPath)) -ArgumentList '-skipNetworkProfileCheck'\n",
"</powershell>"

3) install client packages, ansible
yum -y install python-devel krb5-devel krb5-libs krb5-workstation python-pip gcc

4) pip install "pywinrm>=0.2.2"

5) Verify connectivity to windows server
ansible -i win_hosts_ -m win_ping all

Steps:
1) Set ansible variables:

[windows]
## These are the windows servers
ad1.${GUID}.internal ssh_host=${PUBLIC_ACCESSIBLE_HOSTNAME} ansible_password=${ADMINISTRATOR_PASWORD}

[windows:vars]
ansible_connection=winrm
ansible_port=5986
ansible_ssh_port=5986
ansible_user=Administrator
ansible_ssh_user=Administrator
ansible_winrm_server_cert_validation=ignore
ansible_winrm_transport=basic
ansible_become=false

pip install pywinrm[kerberos]

Run the playbooks

kinit administrator@AD1.${GUID_CAP}.EXAMPLE.OPENTLC.COM
Password for administrator@AD1.${GUID_CAP}.EXAMPLE.OPENTLC.COM:

[root@bastion ~]# klist

