OpenFaaS Templates
------------------

[![OpenFaaS](https://img.shields.io/badge/openfaas-cloud-blue.svg)](https://www.openfaas.com)

## golang-middleware-busybox

This template is mostly the same as [golang-middleware](https://github.com/openfaas/golang-http-template) with the following differences:

1. The final image is `busybox:1.3.2-glibc` instead of `alpine`
2. Any binaries in the `bin` directory relative to the function will be added to the `bin` directory inside 
the working directory of the function, relative to `bin` and be given executable permissions: `chmod +x`.

### Overview

The purpose of using `busybox:glibc` as the base image is to account for some of the 
deficiencies in `alpine` when it comes to DNS resolution.

The copied in binaries from the `bin` directory allow users to use 
`golang` [exec](https://golang.org/pkg/os/exec/) to call the executables from the function handler.

### Example
Please see the basic example below for how to structure your function within the repository.

#### Repository Structure

```
example
├── bin
│   └── ex-bin
└── handler.go
stack.yml
```

#### Golang Handler Snippet

example/handler.go
```go
command := exec.Command("./bin/ex-bin")
```

#### Stack Configuration

stack.yml
```yml
version: 1.0

provider:
  name: openfaas
  gateway: http://127.0.0.1:8080

functions:
  example:
    lang: golang-middleware-busybox
    handler: ./example
    image: example:latest
```