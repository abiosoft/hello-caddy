# hello
This is the beginning of external middlewares for [Caddy](http://caddyserver.com).
Hello is the instruction to start writing your Caddy middleware.

### Quick Start
`go get` this middleware

```shell
$ go get github.com/abiosoft/hello-caddy
```

Run [caddydev](https://github.com/caddyserver/caddydev) to start Caddy with your new middleware.

```shell
$ caddydev -source github.com/abiosoft/hello-caddy hello
Starting caddy...
0.0.0.0:2015
```
Test the middleware

```
$ curl localhost:2015
Hello, I'm a caddy middleware
```

### This is magic, how did it happen ?
By default, Caddy looks for Caddyfile in the current directory and this repository contains a suitable one. **Note** our new directive `hello`.
```
0.0.0.0

hello
```
Then caddydev is started using this repository as source and `hello` as directive.
```
$ caddydev -source github.com/abiosoft/hello-caddy hello
```

#### Digging into the source, hello.go
Firstly, Caddy initializes the middleware using a compatible [Setup function](https://godoc.org/github.com/mholt/caddy/config#SetupFunc). **Note** that the function name must be `Setup`.
```go
func Setup(c *setup.Controller) (middleware.Middleware, error) {
	return func(next middleware.Handler) middleware.Handler {
		return &handler{}
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
