# Get document revisions.

Get document revisions.

## Usage

``` r
db_revisions(cushion, dbname, docid, simplify = TRUE, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  Database name

- docid:

  Document ID

- simplify:

  (logical) Simplify to character vector of revision ids. If `FALSE`,
  gives back availability info too. Default: `TRUE`

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
  db_delete(x, dbname = "sofadb")
}
db_create(x, dbname = "sofadb")

doc1 <- '{"name": "drink", "beer": "IPA", "score": 5}'
doc_create(x, dbname = "sofadb", doc1, docid = "abeer")
doc_create(x, dbname = "sofadb", doc1, docid = "morebeer", as = "json")

db_revisions(x, dbname = "sofadb", docid = "abeer")
db_revisions(x, dbname = "sofadb", docid = "abeer", simplify = FALSE)
db_revisions(x, dbname = "sofadb", docid = "abeer", as = "json")
db_revisions(x, dbname = "sofadb", docid = "abeer", simplify = FALSE, as = "json")
} # }
```
