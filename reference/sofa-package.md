# R client for CouchDB.

Relax.

## About sofa

sofa provides an interface to the NoSQL database CouchDB
(<https://couchdb.apache.org/>). Methods are provided for managing
databases within CouchDB, including
creating/deleting/updating/transferring, and managing documents within
databases. One can connect with a local CouchDB instance, or a remote
CouchDB database such as IBM Cloudant
(<https://www.ibm.com/products/cloudant>). Documents can be inserted
directly from vectors, lists, data.frames, and JSON.

## Client connections

All functions take as their first parameter a client connection object,
or a **cushion**. Create the object with
[Cushion](https://docs.ropensci.org/sofa/reference/Cushion.md). You can
have multiple connection objects in an R session.

## CouchDB versions

sofa was built assuming CouchDB version 2 or greater. Some functionality
of this package will work with versions \< 2, while some may not (mango
queries, see
[`db_query()`](https://docs.ropensci.org/sofa/reference/db_query.md)). I
don't plan to support older CouchDB versions per se.

## Digits after the decimal

If you have any concern about number of digits after the decimal in your
documents, make sure to look at `digits` in your R options. The default
value is 7 (see [options](https://rdrr.io/r/base/options.html) for more
information). You can set the value you like with e.g.,
`options(digits = 10)`, and get what `digits` is set to with
`getOption("digits")`.

Note that in
[`doc_create()`](https://docs.ropensci.org/sofa/reference/doc_create.md)
we convert your document to JSON with
[`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)
if given as a list, which has a `digits` parameter. We pass
`getOption("digits")` to the `digits` parameter in
[`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html).

## Defunct functions

- [attach_get](https://docs.ropensci.org/sofa/reference/attach_get-defunct.md)

## See also

Useful links:

- <https://github.com/ropensci/sofa>

- <https://docs.ropensci.org/sofa/>

- Report bugs at <https://github.com/ropensci/sofa/issues>

## Author

Yaoxiang Li <liyaoxiang@outlook.com>

Scott Chamberlain <myrmecocystus@gmail.com>

Eduard Szöcs <eduardszoecs@gmail.com>
