# Go gotchas

## Where do your pointers go?

Not all pointered variables will get placed on the heap. To see which goes there and which does not, simply run `go build -gcflags="-m"`
