---
layout: page
title: Terraform language functions and snippets
permalink: /newpage/technology/terraform-functions-and-snippets
order: 36
---

This page has wordy crocs; use at your own risk.


```bash
            .-._   _ _ _ _ _ _ _ _
 .-''-.__.-'00  '-' ' ' ' ' ' ' ' '-.
'.___ '    .   .--_'-' '-' '-' _'-' '._
 V: V 'vv-'   '_   '.       .'  _..' '.'.
   '=.____.=_.--'   :_.__.__:_   '.   : :
           (((____.-'        '-.  /   : :
 gator                       (((-'\ .' /
                           _____..'  .'
                          '-._____.-'
                          
                _ ___                /^^\ /^\  /^^\_
    _          _@)@) \            ,,/ '` ~ `'~~ ', `\.
  _/o\_ _ _ _/~`.`...'~\        ./~~..,'`','',.,' '  ~:
 / `,'.~,~.~  .   , . , ~|,   ,/ .,' , ,. .. ,,.   `,  ~\_
( ' _' _ '_` _  '  .    , `\_/ .' ..' '  `  `   `..  `,   \_
 ~V~ V~ V~ V~ ~\ `   ' .  '    , ' .,.,''`.,.''`.,.``. ',   \_
  _/\ /\ /\ /\_/, . ' ,   `_/~\_ .' .,. ,, , _/~\_ `. `. '.,  \_
 < ~ ~ '~`'~'`, .,  .   `_: ::: \_ '      `_/ ::: \_ `.,' . ',  \_
  \ ' `_  '`_    _    ',/ _::_::_ \ _    _/ _::_::_ \   `.,'.,`., \-,-,-,_,_,
   `'~~ `'~~ `'~~ `'~~  \(_)(_)(_)/  `~~' \(_)(_)(_)/ ~'`\_.._,._,'_;_;_;_;_; https // ascii co uk/ art /alligator (unknown)
```

### terraform for_each (instead of count)

```terraform
resource "google_compute_instance" "vm" {
  for_each = {
    "vm1" = "e2-small"
    "vm2" = "e2-medium"
    "vm3" = "f1-micro"
  }

  name         = each.key
  machine_type = each.value
  zone         = "us-central1-a"

  // some additional stuff here if needed
}

resource "google_compute_instance" "vm-multi-value" {
  for_each = {
    "vm1" = { vm_size = "e2-small", zone = "us-central1-a" }
    "vm2" = { vm_size = "e2-medium", zone = "us-central1-b" }
    "vm3" = { vm_size = "f1-micro", zone = "us-central1-c" }
  }

  name         = each.key
  machine_type = each.value.vm_size
  zone         = each.value.zone

  // some additional stuff here if needed
}
```

### alternately terraform for_each with multiple values

```terraform
locals {
  virtual_machines = {
    "vm1" = { vm_size = "e2-small", zone = "us-central1-a" },
    "vm2" = { vm_size = "e2-medium", zone = "us-central1-b" },
    "vm3" = { vm_size = "f1-micro", zone = "us-central1-c" }
  }
}

resource "google_compute_instance" "vm-using-values" {
  for_each = local.virtual_machines
  name = each.key
  machine_type = each.value.vm_size
  zone = each.value.zone
  // some additional stuff here if needed
}
```

### or teraform for_each, multiple values, use in the variables.tf file
```terraform
variable "users" {
  type = list(object({
    name = string
    age  = number
    letter = string
  }))
  default = [
    { name = "person1", age = 20, letter=a },
    { name = "person2", age = 30, letter=b },
    { name = "person3", age = 40, letter=c },
  ]
}

variable "instances" {
  description = "The description"
  type        = map(any)
  default = {
    "008" = { vm_size = "f1-micro", zone="us-central1-b", lengthof="8" }
  }
}

// then use in the something.tf files
resource "google_compute_instance" "vm-using-values-from-variables-dot-tf" {
  for_each = var.instances
  name = each.key
  // name = svr-each.key-random_id.servers
  machine_type = each.value.vm_size
  zone = each.value.zone
  // some additional stuff here if needed
}
```

```terraform
resource "random_id" "servers" {
  keepers = {
    # Generate a new id each time we switch to a new AMI id
    ami_id = var.ami_id
  }
  byte_length = 8
}
// super useful for making unique names like svr-${var.key}-${random_id.servers}
```