# autotls

[![Build Status](https://travis-ci.org/happyjake/autotls.svg?branch=master)](https://travis-ci.org/happyjake/autotls) 
[![Go Report Card](https://goreportcard.com/badge/github.com/happyjake/autotls)](https://goreportcard.com/report/github.com/happyjake/autotls)
[![GoDoc](https://godoc.org/github.com/happyjake/autotls?status.svg)](https://godoc.org/github.com/happyjake/autotls)
[![Join the chat at https://gitter.im/happyjake/autotls](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/happyjake/autotls?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Support Let's Encrypt for a Go server application.

## example

example for 1-line LetsEncrypt HTTPS servers.

[embedmd]:# (example/example1/example1.go go)
```go
package main

import (
	"log"

	"github.com/happyjake/autotls"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	// Ping handler
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong")
	})

	log.Fatal(autotls.Run(r, "example1.com", "example2.com"))
}
```

example for custom autocert manager.

[embedmd]:# (example/example2/example2.go go)
```go
package main

import (
	"log"

	"github.com/happyjake/autotls"
	"github.com/gin-gonic/gin"
	"golang.org/x/crypto/acme/autocert"
)

func main() {
	r := gin.Default()

	// Ping handler
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong")
	})

	m := autocert.Manager{
		Prompt:     autocert.AcceptTOS,
		HostPolicy: autocert.HostWhitelist("example1.com", "example2.com"),
		Cache:      autocert.DirCache("/var/www/.cache"),
	}

	log.Fatal(autotls.RunWithManager(r, &m))
}
```
