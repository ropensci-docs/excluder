# Unite multiple exclusion columns into single column

**\[deprecated\]**

`collapse_exclusions()` was renamed to
[`unite_exclusions()`](https://docs.ropensci.org/excluder/reference/unite_exclusions.md)
to create a more consistent API with tidyverse's `unite()`
function—please use
[`unite_exclusions()`](https://docs.ropensci.org/excluder/reference/unite_exclusions.md).

## Usage

``` r
collapse_exclusions(
  x,
  exclusion_types = c("duplicates", "duration", "ip", "location", "preview", "progress",
    "resolution"),
  separator = ",",
  remove = TRUE
)
```
