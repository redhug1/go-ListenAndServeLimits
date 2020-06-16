# go-ListenAndServeLimits
Results files showing the good effect of limiting the number of go routines launched from serve() in src/net/http/server.go

-=-=-

Add option to Limit maximum number of go routines launched from serve()

Changes to src/net/http/server.go

See:

https://github.com/golang/go/pull/39418

.

and see code changes at:

https://github.com/redhug1/go/commit/ef9da666557ee6cd847b6730f0598db6015f4e2d


The PDF files in this repository show the results of stress testing a web site
that uses ListenAndServe() ...

## e-47.pdf  (no control of memory allocations)
This shows memory allocations with no limits applied to the number of go routines and thus no idea of how many were active at one time.

## e-45.pdf  (good memory profile)
This shows memory allocations with a maximum of 1200 go routines active and responding with a 503 when no more go routines can be launched.

## e-46.pdf  (smallest memory profile)
This shows memory allocations with a maximum of 1200 go routines active and not responding with a 503 when at limit.