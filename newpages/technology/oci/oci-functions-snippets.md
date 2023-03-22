---
layout: page
title: OCI functions and snippets
permalink: /newpage/technology/oci-functions-and-snippets
order: 12
---

This page has wordy crocs; use at your own risk.


```bash

```


```bash

```

```bash
# filename .z-oci-aliases
# source .z-oci-aliases from .zshrc

#IAM
alias oci-avail="oci iam availability-domain list --all | jq '.data[]|(.id,.name)'"

ocit-avail () {
  oci iam availability-domain list --all --query 'data[].{id:"id",name:"name"}' --output=table
}

#IMAGES
function oci-compute-image-list-one-compartment-j-terse () {
  oci compute image list --all --compartment-id="ocid1.compartment.co1..aaa...." | jq =r '[.data[] | {displayName:."display-name" , id: .id}]'
}

# REGIONS
ocit-regions () {
  oci iam region list --output table
}

ocit-regions-phx-profile () {
  oci iam region list --config-file $HOME/.oci/config --profile oci-us-phx-1 --auth security_token
}

# COMPARTMENTS LIST
function oci-compartment-list () {
  oci iam compartment list --query 'data[].{name:"name",id:"id"}'
}

function oci-compartment-list-t () {
  oci iam compartment list --query 'data[].{name:"name",id:"id"}' --output=table
}

function oci-compartment-list-ashburn-frankfurt-sydney-t () {
  regions_in_use="us-ashburn-1 eu-frankfurt-1 ap-sydney-1"
  echo "This command lists compartment IDs for the following in use regions: "
  echo "${regions_in_use}"
  preserve_OCI_CLI_REGION=${OCI_CLI_REGION}
  echo "[*] saving region ${preserve_OCI_CLI_REGION}"
  for i in $(echo ${regions_in_use}); do
    export OCI_CLI_REGION=$1
    echo "[*] Compartments in region $OCI_CLI_REGION"
    oci iam compartment list | jq
  done
  echo "[*] restoring region ${preserve_OCI_CLI_REGION}"
  unset OCI_CLI_REGION

}

# COMPARTMENTS GET ID
function oci-getCompartmentId {
  local compartmentName=$1

  oci iam compartment list --all | jq -r ".data[] | select(.name == \"${compartmentName}\" | .id"
}

# IAM COMPARTMENTS GET ID ALL
function oci-getCompartmentALL-t {
  oci iam compartment list --compartment-id-in-subtree true --query 'data[].{description:"description",name:"name",id:"id"}' --output=table
}


function oci-getCompartmentALL {
  oci iam compartment list --compartment-id-in-subtree true --query 'data[].{description:"description",name:"name",id:"id"}'
}

function ocit-getCompartmentALL-t-terse () {
  oci iam compartment list --compartment-id-in-subtree true --query 'data[].{description:"description",id:"id"}'--output=table
}

# search resources
function oci-showresources-t () {
  oci search resource structured-search --query-text "QUERY all resources where lifeCycleState != 'TERMINATED' && lifeCycleState != 'FAILED'" --query 'data.items[*].{name:"display-name"},resource:"resource-type"}' --output-table
}


function oci-showresources-q () {
  oci search resource structured-search --query-text "QUERY all resources where lifeCycleState != 'TERMINATED' &
& lifeCycleState != 'FAILED'" --query 'data.items[*].{name:"display-name"},resource:"resource-type"}'
}

# NETWORK
function oci-net-subnet-list {
  local ocid_compartment=$1
  local output_type=$2

  my_ocid_compartment="ocid1.compartment.oc1..aaa.....end" 

  oci network subnet list -c ${ocid_compartment} --query 'data[].{cidr_block:"cidr-block","display-name":"display-name",id:"id","prohibit-internet-ingress":"prohibit-internet-ingress","prohibit-public-ip-on-vnic":"prohibit-public-ip-on-vnic",CreatedBy:"defined-tags"."Oracle-Tags"."CreatedBy"}' ${output_type}
}

function oci-net-subnet-list-blame-terse {
  for i in $(oci-getCompartmentALL-t | grep -v description | awk -F\| '{print $3}' | awk '{print $1}'); do echo $i; oci-net-subneet-list $i --output=table; done
}

function oci-net-subnet-list-DEVOPS-t-terse {
  for i in $(oci-getCompartmentALL-t | grep DEVTEST | awk -F\| '{print $3}' | awk '{print $1}'; do oci-net-subnet-list $i --output=table; done
}

function oci-net-subnet-list-DEVTEST-t-terse {
  for i in $(oci-getCompartmentALL-t | grep DEVTEST | awk -F\| '{print $3}' | awk '{print $1}'; do oci-net-subne
t-list $i --output=table; done
}

₩# Network oci network security-list
function oci-net-security-list-egress () {
    echo "Defaults to PROD NETWORK compartment id hardcoded"
    # PROD NETWORK
    COMPARTMENT_ID="ocid1...aaabbbb..ccc"
    
    if [[ -n $1 ]]; then
      COMPARTMENT_ID=$1
    fi
    
    oci network security-list list -c $(COMPARTMENT_ID) --query "data[].\"egress-security-rules\"[].{ip_source:source, p_src_beg:\"tcp-options\".\"source-port-range\".\"min\", p_src_end:\"tcp-options\".\"source-port-range\".\"max\", pd_dest_beg:\"tcp-options\".\"destination-port-range\".\"min\", pd_dest_end:\"tcp-options\".\"destination-port-range\".\"max\", ip_dest:'outbound', z_description:description} | sort_by(@, &ip_dest)" --output=table    
    #oci network security-list list -c $(COMPARTMENT_ID) --query "data[].\"egress-security-rules\"[].{ip_source:source, p_src_beg:\"tcp-options\".\"source-port-range\".\"min\", p_src_end:\"tcp-options\".\"source-port-range\".\"max\", pd_dest_beg:\"tcp-options\".\"destination-port-range\".\"min\", pd_dest_end:\"tcp-options\".\"destination-port-range\".\"max\", ip_dest:'outbound', z_description:description} | sort_by(@[?not_null(pd_dest_end)], &pd_dest_end)" --output=table
}

function oci-net-security-list-ingress () (
  echo "Defaults to PROD NETWORK compartment id hardcoded"
  #PROD NETWORK
  COMPARTMENT_ID="ocid1...aaabbbb..ccc"

  if [[ -n $1 ]]; then
    COMPARTMENT_ID=$1
  fi  

  oci network security-list list -c ${COMPARTMENT_ID} --query "data[].\"ingress-security-rules\"[].{ip_source:source, p_src_beg:\".\"source-port-range\".\"min\", p_src_end:\"tcp-options\".\"source-port-range\".\"max\", pd_dest_beg:\"tcp-options\".\"destination-port-range\".\"min\", pd_dest_end:\"tcp-options\".\"destination-port-range\".\"max\", ip_dest:inbound', z_desc:description} | sort_by(@, &ip_source)" --output=table
}

function oci-net-security-list-ingress-new () {
    echo "Defaults to PROD NETWORK compartment id hardcoded"
    # PROD NETWORK
    COMPARTMENT_ID="ocid1...aaabbbb..ccc"
    
    if [[ -n $1 ]]; then
      COMPARTMENT_ID=$1
    fi   
    

    NUMBER=$(oci network security-list list -c $COMPARTMENT_ID | jq ".[][].\"display-name\" | length " | wc-l)
    #oci network security-list list -c ${COMPARTMENT_ID} | jq '.[][] | "\(."display-name")\t\(.name)"'

    for i in $(seq 0 $((${NUMBER} -1))); do
      SECURITY_LIST_ID=$(oci network security-list list -c ${COMPARTMENT_ID} | jq ".[][2].\"id\"")
      SECURITY_LIST_DN=$(oci network security-list list -c ${COMPARTMENT_ID} | jq ".[][$i].\"display-name\"")
      
      echo "[*] OCID: ${SECURITY_LIST_ID}"
      echo "[*] DISPLAY_NAME: ${SECURITY_LIST_DN}"
      
      oci network security-list list -c ${COMPARTMENT_ID} --query "data[$i].\"ingress-security-rules\"[].{ip_source:source, p_src_beg:\"tcp-options\".\"source-port-range\".\"min\", p_src_end:\"tcp-options\".\"source-port-range\".\"max\", pd_dest_beg:\"tcp-options\".\"destination-port-range\".\"min\", pd_dest_end:\"tcp-options\",\"destination-port-range\".\"max\", ip_dest:'inbound', z_desc:description} | sort_by(@, &ip_source)" --output=table

      echo ""
    done
}

 
# SESSION
oci-session-authenticate () {
  ### Use this command to auth to an new region and update the ~/.oci/config file.
  ### This command will pop a browser window for auth credentials.
  ### This command will update ~/.oci/config file with new auth parameters.
  oci session authenticate
}


# IAM GET TENANCY
function oci-gettenancy {
  oci iam compartment list \
    --all \
    --compartment-id-in-subtree true \
    --access-level ACCESSIBLE \
    --include-root \
    --raw-output \
    --query "data[?contains(\"id\",'tenancy')].id | [0]"
}

### WIP:
# oci network security-list list --compartment-id ${compartment-id}
# oci iam compartment zz-update --description "NEW DESC" --compartment-id "ocid1.compartment.oc1..aaa...."
#
# for i in $(oci-getCompartmentALL-t | grep -v description | awk -F\| '{print $3}' | awk '{print $1}'); do echo $i; oci-net-subnet-list $i; done
# for i in $(oci-getCompartmentALL-t | grep -v description | awk -F\| '{print $3}' | awk '{print $1}'); do echo $i; oci-net-subnet-list $i --output=table; done
#

```
