# Exclude locations outside of US

The `exclude_location()` function removes rows that have locations
outside of the US. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
exclude_location(
  x,
  id_col = "ResponseId",
  location_col = c("LocationLatitude", "LocationLongitude"),
  rename = TRUE,
  include_na = FALSE,
  quiet = TRUE,
  print = TRUE,
  silent = FALSE
)
```

## Arguments

- x:

  Data frame (preferably imported from Qualtrics using {qualtRics}).

- id_col:

  Column name for unique row ID (e.g., participant).

- location_col:

  Two element vector specifying columns for latitude and longitude (in
  that order).

- rename:

  Logical indicating whether to rename columns (using
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md))

- include_na:

  Logical indicating whether to include rows with NA in latitude and
  longitude columns in the output list of potentially excluded data.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

- silent:

  Logical indicating whether to print message to console. Note this
  argument controls the exclude message not the check message.

## Value

An object of the same type as `x` that excludes rows that are located
outside of the US and (if `include_na == FALSE`) rows with no location
information. For a function that checks for these rows, use
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md).
For a function that marks these rows, use
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md).

## Details

To record this information in your Qualtrics survey, you must ensure
that [Anonymize responses is
disabled](https://www.qualtrics.com/support/survey-platform/survey-module/survey-options/survey-protection/#AnonymizingResponses).

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
The function only works for the United States. It uses the \#'
[`maps::map.where()`](https://rdrr.io/pkg/maps/man/map.where.html) to
determine if latitude and longitude are inside the US.

The function outputs to console a message about the number of rows with
locations outside of the US.

## See also

Other location functions:
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md)

Other exclude functions:
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md),
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md),
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md),
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md),
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)

## Examples

``` r
# Exclude locations outside of the US
data(qualtrics_text)
df <- exclude_location(qualtrics_text)
#> ℹ 6 out of 100 rows outside of the US were excluded, leaving 94 rows.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_location()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows outside of the US were excluded, leaving 92 rows.
```
