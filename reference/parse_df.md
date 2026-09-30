# Parse data.frame to json or list by row or column

Parse data.frame to json or list by row or column

## Usage

``` r
parse_df(dat, how = "rows", tojson = TRUE, ...)
```

## Arguments

- dat:

  (data.frame) A data.frame, matrix, or tbl_df

- how:

  (character) One of rows (default) or columns. If rows, each row
  becomes a separate document; if columns, each column becomes a
  separate document.

- tojson:

  (logical) If `TRUE` (default) convert to json - if `FALSE`, to lists

- ...:

  Further args passed on to
  [`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)

## Details

Parse data.frame to get either rows or columns, each as a list or json
string

## Examples

``` r
if (FALSE) { # \dontrun{
parse_df(mtcars, how = "rows")
parse_df(mtcars, how = "columns")
parse_df(mtcars, how = "rows", tojson = FALSE)
parse_df(mtcars, how = "columns", tojson = FALSE)
} # }
```
