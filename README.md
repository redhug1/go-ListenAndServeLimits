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


The PDF files in this repository show the results of stress testing a web site that uses ListenAndServe() ...

The web site being stress tested was showing memory allocations shooting over 80MB. It is preferable for the observed (sampled) peak allocation to stay below 80MB to allow for overshoots of this figure missed in sampling.

The memory allocations were sampled once every 50 with runtime.ReadMemStats() requests to try to minimise the impact of getting the total memory allocated because getting the total memory allocated invloves a process known as Stop The World and it was observed that sampling total memory allocations too often impacted the performance of the web site and also reduced the observed total amount of memory allocated - a problem of the observer affecting what is being observed.

The problem with reduced sampling is that it might miss the true peak allocation, so the values gathered can be used for indication ONLY.

Each PDF contains the details of the test configuration.

## e-47.pdf  (no control of memory allocations, 137MB peak)
This shows memory allocations with no limits applied to the number of go routines and thus no idea of how many were active at one time - a problem due to 'Unbounded parallelism which is rarely a good idea' as detailed in the second paragraph on page 241 of 'The Go Programming Language' - 2016 printed edition.

When a web site is run in a container with limited RAM, the unbounded launching of go routines from within ListenAndServe() can be fatal.



## e-45.pdf  (good memory profile, 67MB peak)
This shows memory allocations with a maximum of 1200 go routines active and responding with a 503 when no more go routines can be launched.

## e-46.pdf  (smallest memory profile, 35MB)
This shows memory allocations with a maximum of 1200 go routines active and not responding with a 503 when at the limit of 1200 go routines.

.

The value of 1200 for the maximum number of go routines was arrived at by trial and error and is only applicable to the web site being tested.