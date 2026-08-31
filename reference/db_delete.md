# Delete a database.

Delete a database.

## Usage

``` r
db_delete(cushion, dbname, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  Database name

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

## Value

JSON as a character string or a list (determined by the `as` parameter)

## Examples

``` r
if (FALSE) { # \dontrun{
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

# local databasees
## create database first, then delete
db_create(x, dbname = "newdb")
db_delete(x, dbname = "newdb")

## with curl info while doing request
library("crul")
db_create(x, "newdb")
db_delete(x, "newdb", verbose = TRUE)
} # }
```
