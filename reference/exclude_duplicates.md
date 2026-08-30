# Exclude rows with duplicate IP addresses and/or locations

The `exclude_duplicates()` function removes rows of data that have the
same IP address and/or same latitude and longitude. The function is
written to work with data from [Qualtrics](https://www.qualtrics.com/)
surveys.

## Usage

``` r
exclude_duplicates(
  x,
  id_col = "ResponseId",
  ip_col = "IPAddress",
  location_col = c("LocationLatitude", "LocationLongitude"),
  rename = TRUE,
  dupl_ip = TRUE,
  dupl_location = TRUE,
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

- ip_col:

  Column name for IP addresses.

- location_col:

  Two element vector specifying columns for latitude and longitude (in
  that order).

- rename:

  Logical indicating whether to rename columns (using
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md))

- dupl_ip:

  Logical indicating whether to check IP addresses.

- dupl_location:

  Logical indicating whether to check latitude and longitude.

- include_na:

  Logical indicating whether to include rows with NAs for IP address and
  location as potentially excluded rows.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

- silent:

  Logical indicating whether to print message to console. Note this
  argument controls the exclude message not the check message.

## Value

An object of the same type as `x` that excludes rows with duplicate IP
addresses and/or locations. For a function that just checks for and
returns duplicate rows, use
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md).
For a function that marks these rows, use
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md).

## Details

To record this information in your Qualtrics survey, you must ensure
that [Anonymize responses is
disabled](https://www.qualtrics.com/support/survey-platform/survey-module/survey-options/survey-protection/#AnonymizingResponses).

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
By default, IP address and location are both checked, but they can be
checked separately with the `dupl_ip` and `dupl_location` arguments.

The function outputs to console separate messages about the number of
rows with duplicate IP addresses and rows with duplicate locations.
These counts are computed independently, so rows may be counted for both
types of duplicates.

## See also

Other duplicates functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md)

Other exclude functions:
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md),
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md),
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md),
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md),
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)

## Examples

``` r
# Exclude duplicate IP addresses and locations
data(qualtrics_text)
df <- exclude_duplicates(qualtrics_text)
#> ℹ 10 out of 100 duplicate rows were excluded, leaving 90 rows.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_duplicates()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 10 out of 98 duplicate rows were excluded, leaving 88 rows.

# Exclude only for duplicate locations
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_duplicates(dupl_location = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 7 out of 98 duplicate rows were excluded, leaving 91 rows.
```
