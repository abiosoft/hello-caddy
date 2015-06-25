# hello
This is the beginning of external middlewares for [Caddy](http://caddyserver.com).
Hello is the instruction to start writing your Caddy middleware.

### Quick Start
`go get` this middleware

```shell
$ go get github.com/abiosoft/hello-caddy
```

Run [caddydev](https://github.com/caddyserver/caddydev) from the directory to start caddy with your middleware.

```shell
$ cd $GOPATH/src/github.com/abiosoft/hello-caddy
$ caddydev
Starting caddy...
0.0.0.0:2015
```
Test the middleware

```
$ curl localhost:2015
Hello, I'm a caddy middleware
```

### This is magic, how did it happen ?
This repository contains a config file `middleware.json` that caddydev needs. It is used to integrate a middleware into caddy.
```json
{
  "name": "Hello",
  "description": "Hello middleware says hello",
  "import": "github.com/abiosoft/hello-caddy",
  "repository": "https://github.com/abiosoft/hello-caddy",
  "directive": "hello"
}
```
It also contains a Caddyfile that enables our new middleware by using the custom directive `hello`.
```
0.0.0.0

hello
```
By default, caddydev looks for config file in the current directory and caddy also looks for Caddyfile in the current directory.

#### Digging into the source, hello.go
Firstly, caddy initializes the middleware using a compatible [Setup function](https://godoc.org/github.com/mholt/caddy/config#SetupFunc).
```go
func Setup(c *setup.Controller) (middleware.Middleware, error) {
	return func(next middleware.Handler) middleware.Handler {
		return handler{}
	}, nil
}
```

Then process requests using the middleware's [Handler](https://godoc.org/github.com/mholt/caddy/middleware#Handler). All that this middleware is doing is to write "Hello, I'm a caddy middleware".
```go
func (h handler) ServeHTTP(w http.ResponseWriter, r *http.Request) (int, error) {
	w.Write([]byte("Hello, I'm a caddy middleware"))
	return 200, nil
}
```

It is not magic afterall ;).
