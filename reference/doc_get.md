# Get a document from a database.

Get a document from a database.

## Usage

``` r
doc_get(
  cushion,
  dbname,
  docid,
  rev = NULL,
  attachments = FALSE,
  deleted = FALSE,
  revs = FALSE,
  revs_info = FALSE,
  conflicts = FALSE,
  deleted_conflicts = FALSE,
  local_seq = FALSE,
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

- docid:

  Document ID

- rev:

  Revision id of the document to get. If NULL, gets current revision

- attachments:

  (logical) Whether to include \_attachments field.

- deleted:

  (logical) Whether to include \_deleted field.

- revs:

  (logical) Whether to include \_revisions field.

- revs_info:

  (logical) Whether to include \_revs_info field.

- conflicts:

  (logical) Whether to include \_conflicts field.

- deleted_conflicts:

  (logical) Whether to include \_deleted_conflicts field.

- local_seq:

  (logical) Whether to include \_local_seq field.

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

# create a document
doc1 <- '{"name": "drink", "type": "drink", "score": 5}'
doc_create(x, dbname = "sofadb", doc1, docid = "asoda")

doc_get(x, dbname = "sofadb", docid = "asoda")
revs <- db_revisions(x, dbname = "sofadb", docid = "asoda")
doc_get(x, dbname = "sofadb", docid = "asoda", rev = revs[1])
doc_get(x, dbname = "sofadb", docid = "asoda", as = "json")
doc_get(x, dbname = "sofadb", docid = "asoda", revs = TRUE)
doc_get(x, dbname = "sofadb", docid = "asoda", revs = TRUE, local_seq = TRUE)
} # }
```
