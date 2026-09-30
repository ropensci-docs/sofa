# Create a new document or update an existing one

Create a new document or update an existing one

## Usage

``` r
doc_upsert(cushion, dbname, doc, docid)
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

## Value

JSON as a character string or a list (determined by the `as` parameter)

## Details

Internally, this function attempts to update a document with the given
name.  
If the document does not exist, it is created

## Author

George Kritikos

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

# create a document
doc1 <- '{"name": "drink", "beer": "IPA", "score": 5}'
doc_upsert(x, dbname = "sofadb", doc1, docid = "abeer")

# update the document
doc2 <- '{"name": "drink", "beer": "lager", "score": 6}'
doc_upsert(x, dbname = "sofadb", doc2, docid = "abeer")


doc_get(x, dbname = "sofadb", docid = "abeer")
} # }
```
