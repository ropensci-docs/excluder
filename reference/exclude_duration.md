# Exclude rows with minimum or maximum durations

The `exclude_duration()` function removes rows of data that have
durations that are too fast or too slow. The function is written to work
with data from [Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
exclude_duration(
  x,
  min_duration = 10,
  max_duration = NULL,
  id_col = "ResponseId",
  duration_col = "Duration (in seconds)",
  rename = TRUE,
  quiet = TRUE,
  print = TRUE,
  silent = FALSE
)
```

## Arguments

- x:

  Data frame (preferably imported from Qualtrics using {qualtRics}).

- min_duration:

  Minimum duration that is too fast in seconds.

- max_duration:

  Maximum duration that is too slow in seconds.

- id_col:

  Column name for unique row ID (e.g., participant).

- duration_col:

  Column name for durations.

- rename:

  Logical indicating whether to rename columns (using
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md))

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

- silent:

  Logical indicating whether to print message to console. Note this
  argument controls the exclude message not the check message.

## Value

An object of the same type as `x` that excludes rows with fast and/or
slow duration. For a function that checks for these rows, use
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md).
For a function that marks these rows, use
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md).

## Details

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
By default, minimum durations of 10 seconds are checked, but either
minima or maxima can be checked with the `min_duration` and
`max_duration` arguments. The function outputs to console separate
messages about the number of rows that are too fast or too slow.

This function returns the fast and slow rows.

## See also

Other duration functions:
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md)

Other exclude functions:
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md),
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md),
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md),
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md),
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)

## Examples

``` r
# Exclude durations faster than 100 seconds
data(qualtrics_text)
df <- exclude_duration(qualtrics_text, min_duration = 100)
#> ℹ 4 out of 100 rows of short and/or long duration were excluded, leaving 96 rows.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_duration()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 0 out of 98 rows of short and/or long duration were excluded, leaving 98 rows.

# Exclude only for durations slower than 800 seconds
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_duration(max_duration = 800)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 2 out of 98 rows of short and/or long duration were excluded, leaving 96 rows.
```
