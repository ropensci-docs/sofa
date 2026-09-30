# Create a database.

Create a database.

## Usage

``` r
db_create(cushion, dbname, delifexists = FALSE, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  Database name

- delifexists:

  If `TRUE`, delete any database of the same name before creating it.
  This is useful for testing. Default: `FALSE`

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

if ("leothetiger" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "leothetiger"))
}
db_create(x, dbname = "leothetiger")

## see if its there now
db_list(x)
} # }
```
