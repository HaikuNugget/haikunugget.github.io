---
layout: page
title: Azure functions and snippets
permalink: /newpage/technology/az-functions-and-snippets
order: 13
---

This page has wordy crocs; use at your own risk.

```bash

```

```bash
function az-account-list-t-terse () {
  output_type="--output=table"
  output_type="$1"
  az account list --query '[].[id, name, state]' --output=table
}

function az-account-tenant-list-t-terse () {
  output_type="--output=table"
  az account tenant list --query '[].{word1hasbugID:id, tenantId:tenantId}' $output_type
}

function az-account-set-subscription () {
  subscriptionID=$1
  az account set --subscription $subscriptionID
}

function az-login-output-tenants-t-terse () {
  TENANT_ID="abc-def-ghi-jkl-mno"
  if [[ -n $1 ]]; then
    TENANT_ID=$1
  fi
  echo "[+] Current tenant ID: $TENANT_ID"
  az login --allow-no-subscriptions --tenant abc-def-ghi-jkl-mno --output table
}

function az-cloud-list-terse () {
  az cloud list --output table
}

function az-vm-show--ssh-publicKeys-terse () {
  resource_group=$1  #RG-GLOBAL
  name=$2 # oci-test-vm
  az vm show --resource-group ${resource_group} --name ${name} --query "osProfile.linuxConfiguration.ssh.publicKeys" | jq '.[].keyData'
}

function az-ad-sp-list-all-t-terse_TAKES3MIN () {
  az-account-list-t-terse
  az ad sp list --all --output=table
}

# AZ NETWORK BASTION
function az-network-bastion-list-t-all () {

  output_type="--output=table"
  if [[ -n $1 ]]; then
    output_type="$1"
  fi

  az network bastion list $output_type
}

function az-network-bastion-list-t-terse () {

  output_type="--output=table"
  if [[ -n $1 ]]; then
    output_type="$1"
  fi

  az network bastion list --query '[].{disCP:disableCopyPaste, DNSName:dnsName, IPConn:enableIpConnect, Name:name, ResourceGrp:resourceGroup}' $output_type
}

function az-network-bastion-list-long-t-terse () {

  output_type="--output=table"
  if [[ -n $1 ]]; then
    output_type="$1"
  fi

  az network bastion list --query '[].{ID:id, disCP:disableCopyPaste, DNSName:dnsName, IPConn:enableIpConnect, Name:name, ResourceGrp:resourceGroup}' $output_type 

}

function az-vm-list-running-terse () {
  az vm list --show-details --query "[?powerState=='VM running'].id" --output tsv
  # az vm list --resource-group VMResources --show-details --query "[?powerState=='VM running'].id" --output tsv

}

```

```bash

function az-ssh-endpoint () {
  # source in separately filename ssh-az-endpoints

  TENANT_ID="from-tenantid-tool-above"
  SUBSCRIPTION_ID="from-subscripiton-id-tool-above"

  az login --tenant $TENANT_ID

  az account set --subscription $SUBSCRIPTION_ID

  sshpass -p "yourpassword" az network bastion ssh --name $name_from_az-network-bastion-long-t-terse \
  --resource-group $resource_group_from_az-network-bastion-long-t-terse \
  --target-resource-id $resource_id-looks-like-a-dir-name_from_az-network-bastoin-long-t-terse \
  --resource-port 22 \
  --port 6000

}
```

```bash
az network lb list --ouptput=table

az network lb show -n lb-name-goes-here-i07fg2ad --resource-group rg-name-goes-here | jq ".backendAddressPools.[0].id"


```

az user information
```bash
az ad user list --filter "userPrincipalName eq 'firstname.lastname@companyname.com'"

az ad signed-in-user show
```

```bash
C:\> echo "windows powershell utility for checksum validation of files"
C:\> CertUtil -hashfile .\filename.txt MD5

C:\> echo "powershell find version of IIS"
C:\> reg query HKLM\SOFTWARE\Microsoft\InetStp
...
VersionString REG_SZ Version 10.0
...

```

```bash
#!/bin/bash

echo "[*] ouput word"
echo "[+] success word"
echo "[-] fail or non-success word"

```

