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

function az-cloud-list-terse () {
  az cloud list --output table
}

function az-vm-show--ssh-publicKeys-terse () {
  resource_group=$1  #RG-GLOBAL
  name=$2 # oci-test-vm
  az vm show --resource-group ${resource_group} --name ${name} --query "osProfile.linuxConfiguration.ssh.publicKeys" | jq '.[].keyData'
}

```

```bash
#!/bin/bash

echo "[*] ouput word"
echo "[+] success word"
echo "[-] fail or non-success word"

```

