# sofa

**An easy interface to CouchDB from R**

sofa docs: <https://docs.ropensci.org/sofa/>

## CouchDB versions

`sofa` works with CouchDB v2 and v3. See [the
builds](https://github.com/ropensci/sofa/actions?query=workflow%3AR-check)
for checks on various CouchDB versions.

## CouchDB Info

- Docs:
  [http://docs.couchdb.org/en/latest/index.html](http://docs.couchdb.org/en/latest/index.md)
- Installation:
  [http://docs.couchdb.org/en/latest/install/index.html](http://docs.couchdb.org/en/latest/install/index.md)

## Connect to CouchDB

This may be starting it on your terminal/shell

``` R
couchdb
```

Or opening the CouchDB app on your machine, or running it in Docker.
Whatever it is, start it up.

## Install sofa

From CRAN

``` R
install.packages("sofa")
```

Development version from GitHub

``` R
remotes::install_github("ropensci/sofa")

library('sofa')
```

## Cushions

Cushions? What? Since it’s couch we gotta use `cushions` somehow.
`cushions` are a connection class containing all connection info to a
CouchDB instance. See
[`?Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md) for
help.

As an example, connecting to a remote CouchDB-compatible service:

``` R
z <- Cushion$new(
  host = "example.com",
  transport = 'https',
  port = NULL,
  user = 'foobar',
  pwd = 'things'
)
```

Break down of parameters:

- `host`: the base url, without the transport (`http`/`https`)
- `path`: context path that is appended to the end of the url
- `transport`: `http` or `https`
- `port`: The port to connect to. Default: 5984. For Cloudant, have to
  set to `NULL`
- `user`: User name for the service.
- `pwd`: Password for the service, if any.
- `headers`: headers to pass in all requests

If you call `Cushion$new()` with no arguments you get a cushion set up
for local use on your machine, with all defaults used.

``` R
x <- Cushion$new()
```

Ping the server

``` R
x$ping()
```

Nice, it’s working.

## More

See the docs <https://docs.ropensci.org/sofa/> for more.

## Meta

- Please [report any issues or
  bugs](https://github.com/ropensci/sofa/issues).
- License: MIT
- Get citation information for `sofa` in R doing
  `citation(package = 'sofa')`
- Please note that this project is released with a [Contributor Code of
  Conduct](https://github.com/ropensci/sofa/blob/master/CODE_OF_CONDUCT.md).
  By participating in this project you agree to abide by its terms.
