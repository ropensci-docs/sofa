# Query a database.

Query a database.

## Usage

``` r
db_query(
  cushion,
  dbname,
  query = NULL,
  selector = NULL,
  limit = NULL,
  skip = NULL,
  sort = NULL,
  fields = NULL,
  use_index = NULL,
  r = NULL,
  bookmark = NULL,
  update = NULL,
  stable = NULL,
  stale = NULL,
  execution_stats = FALSE,
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

- query:

  (character) instead of using the other parameters, you can compose one
  R list or json blob here

- selector:

  (list/json) - JSON object describing criteria used to select
  documents. More information provided in the section on selector
  syntax. See the `query_tutorial` in this package, and the selectors
  docs <https://docs.couchdb.org/en/stable/api/database/find.html>

- limit:

  (numeric) - Maximum number of results returned. Default is 25.
  Optional

- skip:

  (numeric) - Skip the first 'n' results, where 'n' is the value
  specified. Optional

- sort:

  (json) - JSON array following sort syntax. Optional. See
  <https://docs.couchdb.org/en/stable/api/database/find.html> For some
  reason, sort doesn't work often, not sure why.

- fields:

  (json) - JSON array specifying which fields of each object should be
  returned. If it is omitted, the entire object is returned. More
  information provided in the section on filtering fields. Optional See
  <https://docs.couchdb.org/en/stable/api/database/find.html>

- use_index:

  (json) - Instruct a query to use a specific index. Specified either as
  `<design_document>` or `["<design_document>", "<index_name>"]`.
  Optional

- r:

  (numeric) Read quorum needed for the result. This defaults to 1, in
  which case the document found in the index is returned. If set to a
  higher value, each document is read from at least that many replicas
  before it is returned in the results. This is likely to take more time
  than using only the document stored locally with the index. Optional,
  default: 1

- bookmark:

  (character) A string that enables you to specify which page of results
  you require. Used for paging through result sets. Every query returns
  an opaque string under the bookmark key that can then be passed back
  in a query to get the next page of results. If any part of the
  selector query changes between requests, the results are undefined.
  Optional, default: `NULL`

- update:

  (logical) Whether to update the index prior to returning the result.
  Default is true. Optional

- stable:

  (logical) Whether or not the view results should be returned from a
  stable set of shards. Optional

- stale:

  (character) Combination of `update=FALSE` and `stable=TRUE` options.
  Possible options: "ok", "FALSE" (default). Optional

- execution_stats:

  (logical) Include execution statistics in the query response.
  Optional, default: `FALSE`

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
## create a connection
user <- Sys.getenv("COUCHDB_TEST_USER")
pwd <- Sys.getenv("COUCHDB_TEST_PWD")
(x <- Cushion$new(user=user, pwd=pwd))

file <- system.file("examples/omdb.json", package = "sofa")
strs <- readLines(file)

## create a database
if ("omdb" %in% db_list(x)) {
  invisible(db_delete(x, dbname="omdb"))
}
db_create(x, dbname='omdb')

## add some documents
invisible(db_bulk_create(x, "omdb", strs))

## query all in one json blob
db_query(x, dbname = "omdb", query = '{
  "selector": {
    "_id": {
      "$gt": null
    }
  }
}')

## query with each parameter
db_query(x, dbname = "omdb",
  selector = list(`_id` = list(`$gt` = NULL)))

db_query(x, dbname = "omdb",
  selector = list(`_id` = list(`$gt` = NULL)), limit = 3)

# fields
## single field works
db_query(x, dbname = "omdb",
  selector = list(`_id` = list(`$gt` = NULL)), limit = 3,
  fields = c('_id', 'Actors', 'imdbRating'))

## as well as many fields
db_query(x, dbname = "omdb",
  selector = list(`_id` = list(`$gt` = NULL)), limit = 3,
  fields = '_id')

## other queries
db_query(x, dbname = "omdb",
  selector = list(Year = list(`$gt` = "2013")))

db_query(x, dbname = "omdb", selector = list(Rated = "R"))

db_query(x, dbname = "omdb",
  selector = list(Rated = "PG", Language = "English"))

db_query(x, dbname = "omdb", selector = list(
  `$or` = list(
    list(Director = "Jon Favreau"),
    list(Director = "Spike Lee")
  )
), fields = c("_id", "Director"))

## when selector vars are of same name, use a JSON string
## b/c R doesn't let you have a list with same name slots
db_query(x, dbname = "omdb", query = '{
  "selector": {
    "Year": {"$gte": "1990"},
    "Year": {"$lte": "2000"},
    "$not": {"Year": "1998"}
  },
  "fields": ["_id", "Year"]
}')

## regex
db_query(x, dbname = "omdb", selector = list(
  Director = list(`$regex` = "^R")
), fields = c("_id", "Director"))

} # }
```
