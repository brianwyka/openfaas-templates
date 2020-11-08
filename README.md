OpenFaaS Templates
------------------

[![OpenFaaS](https://img.shields.io/badge/openfaas-cloud-blue.svg)](https://www.openfaas.com)

## golang-middleware-busybox

This template is mostly the same as [golang-middleware](https://github.com/openfaas/golang-http-template) with the following differences:

1. The final image is `busybox:1.3.2-glibc` instead of `alpine`
2. Any binaries in the `function/bin` directory will be added to the working directory 
of the function, relative to `bin` and be given executable permissions: `chmod +x`.

### Overview

The purpose of using `busybox:glibc` as the base image is to account for some of the 
deficiencies in `alpine` when it comes to DNS resolution.

The copied in binaries from the `function/bin` directory allow users to use 
`golang` [exec](https://golang.org/pkg/os/exec/) to call the executables from the function handler.

Call the binary `example` from `handler.go`:
```go
command := exec.Command("./bin/example")
```

### Example Stack Configuration

```yml
functions:
  example:
    lang: golang-middleware-busybox
    handler: ./example
    image: example:latest
```