# hello
This is the beginning of external middlewares for [Caddy](http://caddyserver.com).
Hello is the instruction to start writing your Caddy middleware.

### Quick Start
`go get` this middleware

```shell
$ go get https://github.com/abiosoft/hello-caddy
```

Run caddydev from the directory to start caddy with your middleware.

```shell
$ cd $GOPATH/src/github.com/abiosoft/hello-caddy
$ caddydev
Starting caddy...
0.0.0.0:2015
```
Test the middleware

```shell
$ curl localhost:2015
Hello, I'm a caddy middleware
```

### This is magic, how did it happen ?
This repository contains config file `middleware.json`. caddydev needs `middleware.json` to integrate a middleware into caddy.
```json
{
  "name": "Hello",
  "description": "Hello middleware says hello",
  "import": "github.com/abiosoft/hello-caddy",
  "repository": "https://github.com/abiosoft/hello-caddy",
  "directive": "hello",
  "setup": "hello.Setup"
}
```
It also contains a Caddyfile. This is used to test the middleware in action by using the custom directive `hello`.
```
0.0.0.0

hello
```
By default, caddydev looks for config file in the current directory and caddy also looks for Caddyfile in the current directory.

All that this middleware is doing is to write "Hello, I'm a caddy middleware".

From `hello.go`.
```go
func (h handler) ServeHTTP(w http.ResponseWriter, r *http.Request) (int, error) {
	w.Write([]byte("Hello, I'm a caddy middleware"))
	return 200, nil
}
```

It is not magic afterall ;).

### Config
Config | Details
-------|--------
name | Name of the middleware
description | What does your middleware do
import | go import path
repository | middleware repository
directive | keyword to register middleware in Caddyfile
setup | middleware setup function as it would be accessed from external package
after (optional) | priority of middleware. What directive should it be placed after.
