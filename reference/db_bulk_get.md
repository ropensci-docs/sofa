# Query many documents at once

Query many documents at once

## Usage

``` r
db_bulk_get(cushion, dbname, docid_rev, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  (character) Database name. Required.

- docid_rev:

  A list of named lists, each of which must have the slot `id`, and
  optionally `rev` for the revision of the document id

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

## Value

Either a list or json (depending on `as` parameter)

## Examples

``` r
if (FALSE) { # \dontrun{
# initialize a CouchDB connection
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

if ("bulkgettest" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulkgettest"))
}
db_create(x, dbname = "bulkgettest")
db_bulk_create(x, "bulkgettest", mtcars)
res <- db_query(x, dbname = "bulkgettest", selector = list(cyl = 8))

# with ids only
ids <- vapply(res$docs, "[[", "", "_id")
ids_only <- lapply(ids[1:5], function(w) list(id = w))
db_bulk_get(x, "bulkgettest", docid_rev = ids_only)

# with ids and revs
ids_rev <- lapply(
  res$docs[1:3],
  function(w) list(id = w$`_id`, rev = w$`_rev`)
)
db_bulk_get(x, "bulkgettest", docid_rev = ids_rev)
} # }
```
