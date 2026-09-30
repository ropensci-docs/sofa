# Explain API

Explain API

## Usage

``` r
db_explain(
  cushion,
  dbname,
  query = NULL,
  selector = NULL,
  limit = NULL,
  skip = NULL,
  sort = NULL,
  fields = NULL,
  use_index = NULL,
  as = "list",
  ...
)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  Database name

- query:

  (character) instead of using the other parameters, you can compose one
  R list or json blob here

- selector:

  (json) - JSON object describing criteria used to select documents.
  More information provided in the section on selector syntax. See the
  `query_tutorial` in this package, and the selectors docs
  <https://docs.couchdb.org/en/stable/api/database/find.html>

- limit:

  (number) - Maximum number of results returned. Default: 25 Optional

- skip:

  (number) - Skip the first 'n' results, where 'n' is the value
  specified. Optional

- sort:

  (json) - JSON array following sort syntax. Optional. See
  <https://docs.couchdb.org/en/stable/api/database/find.html> For some
  reason, sort doesn't work often, not sure why.

- fields:

  (json) - JSON array specifying which fields of each object should be
  returned. If it is omitted, the entire object is returned. More
  information provided in the section on filtering fields. Optional See
  <https://docs.couchdb.org/en/stable/api/database/find.html>

- use_index:

  (json) - Instruct a query to use a specific index. Specified either as
  `<design_document>` or `["<design_document>", "<index_name>"]`.
  Optional

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
## create a connection
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

file <- system.file("examples/omdb.json", package = "sofa")
strs <- readLines(file)

## create a database
if ("omdb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "omdb"))
}
db_create(x, dbname = "omdb")

## add some documents
invisible(db_bulk_create(x, "omdb", strs))

## query all in one json blob
db_explain(x, dbname = "omdb", query = '{
  "selector": {
    "_id": {
      "$gt": null
    }
  }
}')
} # }
```
