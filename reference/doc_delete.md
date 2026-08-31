# Delete a document in a database.

Delete a document in a database.

## Usage

``` r
doc_delete(cushion, dbname, docid, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  Database name. (character)

- docid:

  Document ID (character)

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

# create a database
if ("sofadb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "sofadb"))
}
db_create(x, dbname = "sofadb")

doc3 <- "<top><a/><b/><c><d/><e>bob</e></c></top>"
doc_create(x, dbname = "sofadb", doc = doc3, docid = "newnewxml")
doc_delete(x, dbname = "sofadb", docid = "newnewxml")

# wrong docid name
doc_create(x, dbname = "sofadb", doc = doc3, docid = "newxml")
# doc_delete(x, dbname="sofadb", docid="wrongname")
} # }
```
