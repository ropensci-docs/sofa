# List database info.

List database info.

## Usage

``` r
db_info(cushion, dbname, as = "list", ...)
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

if ("sofadb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "sofadb"))
}
db_create(x, dbname = "sofadb")

db_info(x, dbname = "sofadb")
db_info(x, dbname = "sofadb", as = "json")
} # }
```
