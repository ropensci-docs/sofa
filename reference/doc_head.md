# Get header info for a document

Get header info for a document

## Usage

``` r
doc_head(cushion, dbname, docid, ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  (character) Database name. Required.

- docid:

  (character) Document ID. Required.

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

# create a database
if ("sofadb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "sofadb"))
}
db_create(x, dbname = "sofadb")

# create a document
doc1 <- '{"name": "drink", "beer": "IPA", "score": 5}'
doc_create(x, dbname = "sofadb", doc1, docid = "abeer")

# run doc_head
doc_head(x, dbname = "sofadb", docid = "abeer")
} # }
```
