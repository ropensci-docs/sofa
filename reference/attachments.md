# Work with attachments

Work with attachments

## Usage

``` r
doc_attach_create(
  cushion,
  dbname,
  docid,
  attachment,
  attname,
  as = "list",
  ...
)

doc_attach_info(cushion, dbname, docid, attname, ...)

doc_attach_get(cushion, dbname, docid, attname = NULL, type = "raw", ...)

doc_attach_delete(cushion, dbname, docid, attname, as = "list", ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  (character) Database name. Required.

- docid:

  (character) Document ID. Required.

- attachment:

  (character) A file name. Required.

- attname:

  (character) Attachment name. Required.

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

- type:

  (character) one of raw (default) or text. required.

## Value

JSON as a character string or a list (determined by the `as` parameter)

## Details

Methods:

- `doc_attach_create` - create an attachment

- `doc_attach_info` - get info (headers) for an attachment

- `doc_attach_get` - get an attachment. this method does not attempt to
  read the object into R, but only gets the raw bytes or plain text. See
  examples for how to read some attachment types

- `doc_attach_delete` - delete and attachment

## Examples

``` r
if (FALSE) { # \dontrun{
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

if ("foodb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "foodb"))
}
db_create(x, dbname = "foodb")

# create an attachment on an existing document
## create a document first
doc <- '{"name":"stuff", "drink":"soda"}'
doc_create(x, dbname = "foodb", doc = doc, docid = "asoda")

## create a csv attachment
row.names(mtcars) <- NULL
file <- tempfile(fileext = ".csv")
write.csv(mtcars, file = file, row.names = FALSE)
doc_attach_create(x,
  dbname = "foodb", docid = "asoda",
  attachment = file, attname = "mtcarstable.csv"
)

## create a binary (png) attachment
file <- tempfile(fileext = ".png")
png(file)
plot(1:10)
dev.off()
doc_attach_create(x,
  dbname = "foodb", docid = "asoda",
  attachment = file, attname = "img.png"
)

## create a binary (pdf) attachment
file <- tempfile(fileext = ".pdf")
pdf(file)
plot(1:10)
dev.off()
doc_attach_create(x,
  dbname = "foodb", docid = "asoda",
  attachment = file, attname = "plot.pdf"
)

# get info for an attachment (HEAD request)
doc_attach_info(x, "foodb", docid = "asoda", attname = "mtcarstable.csv")
doc_attach_info(x, "foodb", docid = "asoda", attname = "img.png")
doc_attach_info(x, "foodb", docid = "asoda", attname = "plot.pdf")

# get an attachment (GET request)
res <- doc_attach_get(x, "foodb",
  docid = "asoda",
  attname = "mtcarstable.csv", type = "text"
)
read.csv(text = res)
doc_attach_get(x, "foodb", docid = "asoda", attname = "img.png")
doc_attach_get(x, "foodb", docid = "asoda", attname = "plot.pdf")
## OR, don't specify an attachment and list the attachments
(attchms <- doc_attach_get(x, "foodb", docid = "asoda", type = "text"))
jsonlite::fromJSON(attchms)

# delete an attachment
doc_attach_delete(x, "foodb", docid = "asoda", attname = "mtcarstable.csv")
doc_attach_delete(x, "foodb", docid = "asoda", attname = "img.png")
doc_attach_delete(x, "foodb", docid = "asoda", attname = "plot.pdf")
} # }
```
