# Mark minimum or maximum durations

The `mark_duration()` function creates a column labeling rows with fast
and/or slow duration. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
mark_duration(
  x,
  min_duration = 10,
  max_duration = NULL,
  id_col = "ResponseId",
  duration_col = "Duration (in seconds)",
  rename = TRUE,
  quiet = FALSE,
  print = TRUE
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

## Value

An object of the same type as `x` that includes a column marking rows
with fast and slow duration. For a function that checks for these rows,
use
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md).
For a function that excludes these rows, use
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md).

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
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md)

Other mark functions:
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md),
[`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md),
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md),
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md),
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md),
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md)

## Examples

``` r
# Mark durations faster than 100 seconds
data(qualtrics_text)
df <- mark_duration(qualtrics_text, min_duration = 100)
#> ℹ 4 out of 100 rows took less time than 100.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  mark_duration()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 0 out of 98 rows took less time than 10.

# Mark only for durations slower than 800 seconds
df <- qualtrics_text %>%
  exclude_preview() %>%
  mark_duration(max_duration = 800)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 0 out of 98 rows took less time than 10.
#> ℹ 2 out of 98 rows took more time than 800.
```
