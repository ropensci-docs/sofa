# Work with design documents

Work with design documents

## Usage

``` r
design_create(
  cushion,
  dbname,
  design,
  fxnname,
  key = "null",
  value = "doc",
  as = "list",
  ...
)

design_create_(cushion, dbname, design, fxnname, fxn, as = "list", ...)

design_delete(cushion, dbname, design, as = "list", ...)

design_get(cushion, dbname, design, as = "list", ...)

design_head(cushion, dbname, design, ...)

design_info(cushion, dbname, design, ...)
```

## Arguments

- cushion:

  A [`Cushion`](https://docs.ropensci.org/sofa/reference/Cushion.md)
  object. Required.

- dbname:

  (character) Database name. required.

- design:

  (character) Design document name. this is the design name without
  `_design/`, which is prepended internally. required.

- fxnname:

  (character) A function name. Required for `design_create` and
  `design_create_`.

- key, value:

  (character) a key and value, see Examples and Details

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

- fxn:

  (character) a javascript function. Required for `design_create_`.

## Value

JSON as a character string or a list (determined by the `as` parameter)

## Details

`design_create` is a slightly easier interface to creating design
documents; it just asks for a function name, the key and a value, then
we create the function for you internally. To have more flexibility use
`view_put_` (with underscore on the end) to write the function yourself.

## Examples

``` r
if (FALSE) { # \dontrun{
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user = user, pwd = pwd))

file <- system.file("examples/omdb.json", package = "sofa")
strs <- readLines(file)

## create a database
if ("omdb" %in% db_list(x)) {
  invisible(db_delete(x, dbname = "omdb"))
}
db_create(x, dbname = "omdb")

## add the documents
invisible(db_bulk_create(x, "omdb", strs))

# Create a view, the easy way, but less flexible
design_create(x, dbname = "omdb", design = "view1", fxnname = "foobar1")
design_create(x,
  dbname = "omdb", design = "view2", fxnname = "foobar2",
  value = "doc.Country"
)
design_create(x,
  dbname = "omdb", design = "view5", fxnname = "foobar3",
  value = "[doc.Country,doc.imdbRating]"
)

# the harder way, write your own function, but more flexible
design_create_(x,
  dbname = "omdb", design = "view22",
  fxnname = "stuffthings", fxn = "function(doc){emit(null,doc.Country)}"
)

# Delete a view
design_delete(x, dbname = "omdb", design = "view1")

# Get info on a design document
## HEAD request, returns just response headers
design_head(x, dbname = "omdb", design = "view2")
design_head(x, dbname = "omdb", design = "view5")
## GET request, returns information about the design document
design_info(x, dbname = "omdb", design = "view2")
design_info(x, dbname = "omdb", design = "view5")

# Get a design document (GET request)
design_get(x, dbname = "omdb", design = "view2")
design_get(x, dbname = "omdb", design = "view5")

# Search using a view
res <- design_search(x, dbname = "omdb", design = "view2", view = "foobar2")
head(
  do.call(
    "rbind.data.frame",
    lapply(res$rows, function(x) Filter(length, x))
  )
)

res <- design_search(x, dbname = "omdb", design = "view5", view = "foobar3")
head(
  structure(do.call(
    "rbind.data.frame",
    lapply(res$rows, function(x) x$value)
  ), names = c("Country", "imdbRating"))
)
} # }
```
