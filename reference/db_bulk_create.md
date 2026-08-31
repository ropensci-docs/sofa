# Create documents via the bulk API

Create documents via the bulk API

## Usage

``` r
db_bulk_create(
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

  A data.frame, list, or JSON as a character string. Required.

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

## Details

Note that row.names are dropped from data.frame inputs.

## Examples

``` r
if (FALSE) { # \dontrun{
# initialize a CouchDB connection
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

# From a data.frame
if ("bulktest" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulktest"))
}
db_create(x, dbname = "bulktest")
db_bulk_create(x, "bulktest", mtcars)

if ("bulktest2" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulktest2"))
}
db_create(x, dbname = "bulktest2")
db_bulk_create(x, "bulktest2", iris)

# data.frame with 1 or more columns as neseted lists
mtcars$stuff <- list("hello_world")
mtcars$stuff2 <- list("hello_world", "things")
if ("bulktest3" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulktest3"))
}
db_create(x, dbname = "bulktest3")
db_bulk_create(x, "bulktest3", mtcars)

# From a json character string, or more likely, many json character strings
library("jsonlite")
strs <- as.character(parse_df(mtcars, "columns"))
if ("bulkfromchr" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulkfromchr"))
}
db_create(x, dbname = "bulkfromchr")
db_bulk_create(x, "bulkfromchr", strs)

# From a list of lists
library("jsonlite")
lst <- parse_df(mtcars, tojson = FALSE)
if ("bulkfromchr" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "bulkfromchr"))
}
db_create(x, dbname = "bulkfromchr")
db_bulk_create(x, "bulkfromchr", lst)

# iris dataset - by rows
if ("irisrows" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "irisrows"))
}
db_create(x, dbname = "irisrows")
db_bulk_create(x, "irisrows", apply(iris, 1, as.list))

# iris dataset - by columns - doesn't quite work yet
# if ("iriscolumns" %in% db_list(x)) {
#   invisible(db_delete(x, dbname="iriscolumns"))
# }
# db_create(x, dbname="iriscolumns")
# db_bulk_create(x, "iriscolumns", parse_df(iris, "columns", tojson=FALSE), how="columns")
} # }
```
