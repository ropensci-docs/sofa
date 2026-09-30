# Update a document.

Update a document.

## Usage

``` r
doc_update(cushion, dbname, doc, docid, rev, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  (character) Database name. Required.

- doc:

  (character) Document content. Required.

- docid:

  (character) Document ID. Required.

- rev:

  (character) Revision id. Required.

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

## Value

JSON as a character string or a list (determined by the `as` parameter)

## Details

Internally, this function adds in the docid and revision id, required to
do a document update

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

doc1 <- '{"name":"drink","beer":"IPA"}'
doc_create(x, dbname = "sofadb", doc = doc1, docid = "b_beer")
doc_get(x, dbname = "sofadb", docid = "b_beer")
revs <- db_revisions(x, dbname = "sofadb", docid = "b_beer")
doc2 <- '{"name":"drink","beer":"IPA","note":"yummy","note2":"yay"}'
doc_update(x, dbname = "sofadb", doc = doc2, docid = "b_beer", rev = revs[1])
db_revisions(x, dbname = "sofadb", docid = "b_beer")
} # }
```
