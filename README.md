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

The web site being stress tested was showing memory allocations shooting over 80MB. It is preferable for the observed (sampled) peak allocation to stay below 80MB to allow for overshoots of this figure missed in sampling.

The memory allocations were sampled once every 50 requests to try to minimise the impact of getting the total memory allocated because getting the total memory allocated invloves a process known as Stop The World and it was observed that sampling total memory allocations too often impacted the performance of the web site and also reduced the observed total amount of memory allocated - a problem of the observer affecting what is being observed.

The problem with reduced sampling is that it might miss the true peak allocation, so the values gathered can be used for indication ONLY.

## e-47.pdf  (no control of memory allocations, 137MB peak)
This shows memory allocations with no limits applied to the number of go routines and thus no idea of how many were active at one time.

## e-45.pdf  (good memory profile, 67MB peak)
This shows memory allocations with a maximum of 1200 go routines active and responding with a 503 when no more go routines can be launched.

## e-46.pdf  (smallest memory profile, 35MB)
This shows memory allocations with a maximum of 1200 go routines active and not responding with a 503 when at limit.