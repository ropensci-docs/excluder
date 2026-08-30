# Mark locations outside of US

The `mark_location()` function creates a column labeling rows that have
locations outside of the US. The function is written to work with data
from [Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
mark_location(
  x,
  id_col = "ResponseId",
  location_col = c("LocationLatitude", "LocationLongitude"),
  rename = TRUE,
  include_na = FALSE,
  quiet = FALSE,
  print = TRUE
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

## Value

An object of the same type as `x` that includes a column marking rows
that are located outside of the US and (if `include_na == FALSE`) rows
with no location information. For a function that checks for these rows,
use
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md).
For a function that excludes these rows, use
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md).

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
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md)

Other mark functions:
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md),
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md),
[`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md),
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md),
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md),
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md)

## Examples

``` r
# Mark locations outside of the US
data(qualtrics_text)
df <- mark_location(qualtrics_text)
#> ℹ 1 out of 100 rows had no information on location.
#> ℹ 5 out of 100 rows were located outside of the US.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  mark_location()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 1 out of 98 rows had no information on location.
#> ℹ 5 out of 98 rows were located outside of the US.
```
