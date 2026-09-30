# sofa connection client

sofa connection client

sofa connection client

## Value

An object of class `Cushion`, with variables accessible for host, port,
path, transport, user, pwd, and headers. Functions are callable to get
headers, and to make the base url sent with all requests.

## CouchDB versions

sofa was built assuming CouchDB version 2 or greater. Some functionality
of this package will work with versions \< 2, while some may not (mango
queries, see
[`db_query()`](https://docs.ropensci.org/sofa/reference/db_query.md)). I
don't plan to support older CouchDB versions per se.

## Public fields

- `host`:

  (character) host

- `port`:

  (integer) port

- `path`:

  (character) url path, if any

- `transport`:

  (character) transport schema, (http\|https)

- `user`:

  (character) username

- `pwd`:

  (character) password

- `headers`:

  (list) named list of headers

## Methods

### Public methods

- [`Cushion$new()`](#method-Cushion-new)

- [`Cushion$print()`](#method-Cushion-print)

- [`Cushion$ping()`](#method-Cushion-ping)

- [`Cushion$make_url()`](#method-Cushion-make_url)

- [`Cushion$get_headers()`](#method-Cushion-get_headers)

- [`Cushion$get_auth()`](#method-Cushion-get_auth)

- [`Cushion$version()`](#method-Cushion-version)

- [`Cushion$clone()`](#method-Cushion-clone)

------------------------------------------------------------------------

### Method `new()`

Create a new `Cushion` object

#### Usage

    Cushion$new(host, port, path, transport, user, pwd, headers)

#### Arguments

- `host`:

  (character) A base URL (without the transport), e.g., `localhost`,
  `127.0.0.1`, or `foobar.cloudant.com`

- `port`:

  (numeric) Port. Remember that if you don't want a port set, set this
  parameter to `NULL`. Default: `5984`

- `path`:

  (character) context path that is appended to the end of the url. e.g.,
  `bar` in `http://foo.com/bar`. Default: `NULL`, ignored

- `transport`:

  (character) http or https. Default: http

- `user, pwd`:

  (character) user name, and password. these are used in all requests.
  if absent, they are not passed to requests

- `headers`:

  A named list of headers. These headers are used in all requests. To
  use headers in individual requests and not others, pass in headers via
  `...` in a function call.

#### Returns

A new `Cushion` object

------------------------------------------------------------------------

### Method [`print()`](https://rdrr.io/r/base/print.html)

print method for `Cushion`

#### Usage

    Cushion$print()

#### Arguments

- `x`:

  self

- `...`:

  ignored

------------------------------------------------------------------------

### Method [`ping()`](https://docs.ropensci.org/sofa/reference/ping.md)

Ping the CouchDB server

#### Usage

    Cushion$ping(as = "list", ...)

#### Arguments

- `as`:

  (character) One of list (default) or json

- `...`:

  curl options passed to
  [crul::verb-GET](https://docs.ropensci.org/crul/reference/verb-GET.html)

------------------------------------------------------------------------

### Method `make_url()`

Construct full base URL from the pieces in the connection object

#### Usage

    Cushion$make_url()

------------------------------------------------------------------------

### Method `get_headers()`

Get list of headers that will be sent with each request

#### Usage

    Cushion$get_headers()

------------------------------------------------------------------------

### Method `get_auth()`

Get list of auth values, user and pwd

#### Usage

    Cushion$get_auth()

------------------------------------------------------------------------

### Method [`version()`](https://rdrr.io/r/base/Version.html)

Get the CouchDB version as a numeric

#### Usage

    Cushion$version()

------------------------------------------------------------------------

### Method `clone()`

The objects of this class are cloneable with this method.

#### Usage

    Cushion$clone(deep = FALSE)

#### Arguments

- `deep`:

  Whether to make a deep clone.

## Examples

``` r
if (FALSE) { # \dontrun{
# Create a CouchDB connection client
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

## metadata
x$host
x$path
x$port
x$type

## ping the CouchDB server
x$ping()

## get CouchDB version
x$version()

# create database
if (!"stuff" %in% db_list(x)) {
  db_create(x, "stuff")
}

# add documents to a database
if (!"sofadb" %in% db_list(x)) {
  db_create(x, "sofadb")
}
doc1 <- '{"name": "drink", "beer": "IPA", "score": 5}'
doc_create(x, dbname = "sofadb", docid = "abeer", doc1)

# bulk create
if (!"mymtcars" %in% db_list(x)) {
  db_create(x, "mymtcars")
}
db_bulk_create(x, dbname = "mymtcars", doc = mtcars)
db_list(x)

## database info
db_info(x, "mymtcars")

## list dbs
db_list(x)

## all docs
db_alldocs(x, "mymtcars", limit = 3)

## changes
db_changes(x, "mymtcars")

# With auth
# x <- Cushion$new(user = 'sckott', pwd = 'sckott')

# Using Cloudant
# z <- Cushion$new(host = "ropensci.cloudant.com", transport = 'https', port = NULL,
#   user = 'ropensci', pwd = Sys.getenv('CLOUDANT_PWD'))
# z
# db_list(z)
# db_create(z, "stuff2")
# db_info(z, "stuff2")
# db_alldocs(z, "foobar")
} # }
```
