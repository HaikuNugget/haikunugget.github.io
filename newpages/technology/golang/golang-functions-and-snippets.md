---
layout: page
title: Go language functions and snippets
permalink: /newpage/technology/golang-functions-and-snippets
order: 32
---

This page has wordy crocs; use at your own risk.


```bash

```

#### generate a list for use with a terraform for_each variable
compile with GOOS="linux" GOARCH="amd64" -o filname-x86 ./filename.go
```go
package main

import (
        "flag"
        "fmt"
        "os"
        "path/filepath"
)

/* This program is used to quickly generate a map of machines for use in the XXX variable in terraform. */
func main() {

        var counterA = 1
        var (
                flStartcount = flag.Int("s", 1, "The number of machines at which to start counting.")
                flEndcount   = flag.Int("e", 15, "The number of machines at which to end counting.")
        )
        flag.Parse()

        if *flStartcount > *flEndcount {
                fmt.Fprintf(os.Stderr, "The start value %d is larger than the ending value of %d.\n", *flStartcount, *flEndcount)
                fmt.Fprintf(os.Stderr, "Did you intend: %s -s %d -e %d ?\n", filepath.Base(os.Args[0]), *flEndcount, *flStartcount)
                os.Exit(15)
        }

        for i := *flStartcount; i <= *flEndcount; i++ {
                s := fmt.Sprintf("%03d", i)
                fmt.Fprintf(os.Stdout, "{ var1=%s, var2=%s, var3=%d },\n", s, s, counterA)
        }
}

/*
// Example terraform variable.tf declaration and initialization:

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
*/
```
