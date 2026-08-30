# Unite multiple exclusion columns into single column

Each of the `mark_*()` functions appends a new column to the data. The
`unite_exclusions()` function unites all of those columns in a single
column that can be used to filter any or all exclusions downstream. Rows
with multiple exclusions are concatenated with commas.

## Usage

``` r
unite_exclusions(
  x,
  exclusion_types = c("duplicates", "duration", "ip", "location", "preview", "progress",
    "resolution"),
  separator = ",",
  remove = TRUE
)
```

## Arguments

- x:

  Data frame or tibble (preferably exported from Qualtrics).

- exclusion_types:

  Vector of types of exclusions to unite.

- separator:

  Character string specifying what character to use to separate multiple
  exclusion types

- remove:

  Logical specifying whether to remove united columns (default = TRUE)
  or leave them in the data frame (FALSE)

## Value

An object of the same type as `x` that includes the all of the same rows
but with a single `exclusion` column replacing all of the specified
`exclusion_*` columns.

## Examples

``` r

# Unite all exclusion types
df <- qualtrics_text %>%
  mark_duplicates() %>%
  mark_duration(min_duration = 100) %>%
  mark_ip() %>%
  mark_location() %>%
  mark_preview() %>%
  mark_progress() %>%
  mark_resolution()
#> ℹ 2 NAs were found in IP addresses.
#> ℹ 7 out of 7 rows had duplicate IP addresses.
#> ℹ 1 NA was found in location.
#> ℹ 10 out of 10 rows had duplicate locations.
#> ℹ 4 out of 100 rows took less time than 100.
#> ℹ 2 out of 100 rows had NA values for IP addresses (check for preview data with 'check_preview()').
#> ℹ 18 out of 100 rows had IP address outside of US.
#> ℹ 1 out of 100 rows had no information on location.
#> ℹ 5 out of 100 rows were located outside of the US.
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#> ℹ 6 out of 100 rows did not complete the study.
#> ℹ 3 out of 100 rows had screen resolutions less than 0 or height less than 0.
df2 <- df %>%
  unite_exclusions()

# Unite subset of exclusion types
df2 <- df %>%
  unite_exclusions(exclusion_types = c("duplicates", "duration", "ip"))
```
