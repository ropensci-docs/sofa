# Create documents via the bulk API

Create documents via the bulk API

## Usage

``` r
db_bulk_update(
  cushion,
  dbname,
  doc,
  docid = NULL,
  how = "rows",
  as = "list",
  ...
)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  (character) Database name. Required.

- doc:

  For now, a data.frame only. Required.

- docid:

  Document IDs, ignored for now, eventually, you can pass in a list, or
  vector to be the ids for each document created. Has to be the same
  length as the number of documents.

- how:

  (character) One of rows (default) or columns. If rows, each row
  becomes a separate document; if columns, each column becomes a
  separate document.

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

## Value

Either a list or json (depending on `as` parameter), with each element
an array of key:value pairs:

- ok - whether creation was successful

- id - the document id

- rev - the revision id

## Examples

``` r
if (FALSE) { # \dontrun{
# initialize a CouchDB connection
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

row.names(mtcars) <- NULL

if ("bulktest" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulktest"))
}
db_create(x, dbname = "bulktest")
db_bulk_create(x, mtcars, dbname = "bulktest")

# modify mtcars
mtcars$letter <- sample(letters, NROW(mtcars), replace = TRUE)
db_bulk_update(x, "bulktest", mtcars)

# change again
mtcars$num <- 89
db_bulk_update(x, "bulktest", mtcars)
} # }
```
