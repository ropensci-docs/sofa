# Search design documents

Search design documents

## Usage

``` r
design_search(
  cushion,
  dbname,
  design,
  view,
  params = list(),
  body = list(),
  as = "list",
  ...
)

design_search_many(cushion, dbname, design, view, queries, as = "list", ...)
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

- view:

  (character) a view, same as `fxn` param in
  [`design_create_()`](https://docs.ropensci.org/sofa/reference/design.md).
  required.

- params:

  query parameters. a named list

- body:

  same as `params`, but if any given, a POST request is sent (if body
  non-NULL, `params` also sent). a named list

- as:

  (character) One of list (default) or json

- ...:

  Curl args passed on to
  [`HttpClient`](https://docs.ropensci.org/crul/reference/HttpClient.html)

- queries:

  a list of named lists of queries

## Value

JSON as a character string or a list (determined by the `as` parameter)

## Options to pass to `params`, `body`, or `queries` params

- conflicts (logical) Includes conflicts information in response.
  Ignored if include_docs isn't `TRUE`. Default: `FALSE`

- descending (logical) Return the documents in descending by key order.
  Default: `FALSE`

- endkey,end_key (list) Stop returning records when the specified key is
  reached. Optional. `end_key` is an alias for `endkey`

- endkey_docid,end_key_doc_id (character) Stop returning records when
  the specified document ID is reached. Requires endkey to be specified
  for this to have any effect. Optional. `end_key_doc_id` is an alias
  for `endkey_docid`

- group (logical) Group the results using the reduce function to a group
  or single row. Default: `FALSE`

- group_level (integer) Specify the group level to be used. Optional

- include_docs (logical) Include the associated document with each row.
  Default: `FALSE`.

- attachments (logical) Include the Base64-encoded content of
  attachments in the documents that are included if include_docs is
  `TRUE`. Ignored if include_docs isn't `TRUE`. Default: `FALSE`

- att_encoding_info (logical) Include encoding information in attachment
  stubs if include_docs is `TRUE` and the particular attachment is
  compressed. Ignored if include_docs isn't `TRUE`. Default: `FALSE`.

- inclusive_end (logical) Specifies whether the specified end key should
  be included in the result. Default: `TRUE`

- key (list) Return only documents that match the specified key.
  Optional

- keys (list) Return only documents where the key matches one of the
  keys specified in the array. Optional

- limit (integer) Limit the number of the returned documents to the
  specified number. Optional

- reduce (logical) Use the reduction function. Default: `TRUE`

- skip (integer) Skip this number of records before starting to return
  the results. Default: 0

- sorted (logical) Sort returned rows (see Sorting Returned Rows).
  Setting this to `FALSE` offers a performance boost. The total_rows and
  offset fields are not available when this is set to `FALSE`. Default:
  `TRUE`

- stale (character) Allow the results from a stale view to be used.
  Supported values: ok and update_after. Optional

- startkey,start_key (list) Return records starting with the specified
  key. Optional. `start_key` is an alias for startkey

- startkey_docid,start_key_doc_id (character) Return records starting
  with the specified document ID. Requires startkey to be specified for
  this to have any effect. Optional. `start_key_doc_id` is an alias for
  `startkey_docid`

- update_seq (logical) Response includes an update_seq value indicating
  which sequence id of the database the view reflects. Default: `FALSE`

## References

https://docs.couchdb.org/en/latest/api/ddoc/views.html

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
design_create_(x,
  dbname = "omdb", design = "view6", fxnname = "foobar4",
  fxn = "function(doc){emit(doc._id,doc.Country)}"
)

# Search using a view
compact <- function(l) Filter(Negate(is.null), l)
res <- design_search(x, dbname = "omdb", design = "view2", view = "foobar2")
head(
  do.call(
    "rbind.data.frame",
    Filter(
      function(z) length(z) == 2,
      lapply(res$rows, function(x) compact(x[names(x) %in% c("id", "value")]))
    )
  )
)

res <- design_search(x, dbname = "omdb", design = "view5", view = "foobar3")
head(
  structure(do.call(
    "rbind.data.frame",
    lapply(res$rows, function(x) x$value)
  ), names = c("Country", "imdbRating"))
)

# query parameters
## limit
design_search(x,
  dbname = "omdb", design = "view5", view = "foobar3",
  params = list(limit = 5)
)
## limit and skip
design_search(x,
  dbname = "omdb", design = "view5", view = "foobar3",
  params = list(limit = 5, skip = 3)
)
## with start and end keys
### important: the key strings have to be in JSON, so here e.g.,
###  need to add escaped double quotes
res <- design_search(
  cushion = x,
  dbname = "omdb",
  design = "view6",
  view = "foobar4",
  params = list(
    startkey = "\"c25bbf4fef99408b3e1115374a03f642\"",
    endkey = "\"c25bbf4fef99408b3e1115374a040f11\""
  )
)

# POST request
ids <- vapply(db_alldocs(x, dbname = "omdb")$rows[1:3], "[[", "", "id")
res <- design_search(x,
  dbname = "omdb", design = "view6", view = "foobar4",
  body = list(keys = ids), verbose = TRUE
)
res

# Many queries at once in a POST request
queries <- list(
  list(keys = ids),
  list(limit = 3, skip = 2)
)
design_search_many(x, "omdb", "view6", "foobar4", queries)
} # }
```
