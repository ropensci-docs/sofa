# Create documents to a database.

Create documents to a database.

## Usage

``` r
doc_create(cushion, dbname, doc, docid = NULL, how = "rows", as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  Database name3

- doc:

  Document content, can be character string or a list. The character
  type can be XML as well, if embedded in JSON. When the document is
  retrieved via
  [`doc_get()`](https://docs.ropensci.org/sofa/reference/doc_get.md),
  the XML is given back and you can parse it as normal.

- docid:

  Document ID

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

JSON as a character string or a list (determined by the `as` parameter)

## Details

Documents can have attachments just like email. There are two ways to
use attachments: the first one is via a separate REST call (see
[`doc_attach_create()`](https://docs.ropensci.org/sofa/reference/attachments.md));
the second is inline within your document, you can do so with this
function. See
https://docs.couchdb.org/en/latest/api/document/attachments.html for
help on formatting json appropriately.

Note that you can create documents from a data.frame with this function,
where each row or column is a separate document. However, this function
does not use the bulk API
<https://docs.couchdb.org/en/stable/api/database/bulk-api.html#db-bulk-docs>

- see
  [`db_bulk_create()`](https://docs.ropensci.org/sofa/reference/db_bulk_create.md)
  and
  [`db_bulk_update()`](https://docs.ropensci.org/sofa/reference/db_bulk_update.md)
  to create or update documents with the bulk API - which should be much
  faster for a large number of documents.

## Digits after the decimal

If you have any concern about number of digits after the decimal in your
documents, make sure to look at `digits` in your R options. The default
value is 7 (see [options](https://rdrr.io/r/base/options.html) for more
information). You can set the value you like with e.g.,
`options(digits = 10)`, and get what `digits` is set to with
`getOption("digits")`.

Note that in `doc_create()` we convert your document to JSON with
[`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)
if given as a list, which has a `digits` parameter. We pass
`getOption("digits")` to the `digits` parameter in
[`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)

## Examples

``` r
if (FALSE) { # \dontrun{
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

if ("sofadb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "sofadb"))
}
db_create(x, "sofadb")

# write a document WITH a name (uses PUT)
doc1 <- '{"name": "drink", "beer": "IPA", "score": 5}'
doc_create(x, dbname = "sofadb", doc1, docid = "abeer")
doc_create(x, dbname = "sofadb", doc1, docid = "morebeer", as = "json")
doc_get(x, dbname = "sofadb", docid = "abeer")
## with factor class values
doc2 <- list(name = as.factor("drink"), beer = "stout", score = 4)
doc_create(x, doc2, dbname = "sofadb", docid = "nextbeer", as = "json")
doc_get(x, dbname = "sofadb", docid = "nextbeer")

# write a json document WITHOUT a name (uses POST)
doc2 <- '{"name": "food", "icecream": "rocky road"}'
doc_create(x, doc2, dbname = "sofadb")
doc3 <- '{"planet": "mars", "size": "smallish"}'
doc_create(x, doc3, dbname = "sofadb")
## assigns a UUID instead of a user given name
db_alldocs(x, dbname = "sofadb")

# write an xml document WITH a name (uses PUT). xml is written as xml in
# couchdb, just wrapped in json, when you get it out it will be as xml
doc4 <- "<top><a/><b/><c><d/><e>bob</e></c></top>"
doc_create(x, doc4, dbname = "sofadb", docid = "somexml")
doc_get(x, dbname = "sofadb", docid = "somexml")

# You can pass in lists that autoconvert to json internally
doc1 <- list(name = "drink", type = "soda", score = 9)
doc_create(x, dbname = "sofadb", doc1, docid = "gooddrink")

# Write directly from a data.frame
## Each row or column becomes a separate document
### by rows
if ("test" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "test"))
}
db_create(x, dbname = "test")
doc_create(x, mtcars, dbname = "test", how = "rows")
doc_create(x, mtcars, dbname = "test", how = "columns")

if ("testiris" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "testiris"))
}
db_create(x, dbname = "testiris")
head(iris)
doc_create(x, iris, dbname = "testiris")
} # }
```
