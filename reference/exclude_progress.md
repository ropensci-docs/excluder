# Exclude survey progress

The `exclude_progress()` function removes rows that have incomplete
progress. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
exclude_progress(
  x,
  min_progress = 100,
  id_col = "ResponseId",
  finished_col = "Finished",
  progress_col = "Progress",
  rename = TRUE,
  quiet = TRUE,
  print = TRUE,
  silent = FALSE
)
```

## Arguments

- x:

  Data frame (preferably imported from Qualtrics using {qualtRics}).

- min_progress:

  Amount of progress considered acceptable to include.

- id_col:

  Column name for unique row ID (e.g., participant).

- finished_col:

  Column name for whether survey was completed.

- progress_col:

  Column name for percentage of survey completed.

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

An object of the same type as `x` that excludes rows that have
incomplete progress. For a function that checks for these rows, use
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md).
For a function that marks these rows, use
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md).

## Details

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
The default requires 100% completion, but lower levels of completion
maybe acceptable and can be allowed by specifying the `min_progress`
argument. The finished column in Qualtrics can be a numeric or character
vector depending on whether it is exported as choice text or numeric
values. This function works for both.

The function outputs to console a message about the number of rows that
have incomplete progress.

## See also

Other progress functions:
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md),
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md)

Other exclude functions:
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md),
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md),
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md),
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md),
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)

## Examples

``` r
# Exclude rows with incomplete progress
data(qualtrics_text)
df <- exclude_progress(qualtrics_text)
#> ℹ 6 out of 100 rows with incomplete progress were excluded, leaving 94 rows.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_progress()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows with incomplete progress were excluded, leaving 92 rows.

# Include a lower acceptable completion percentage
df <- qualtrics_numeric %>%
  exclude_preview() %>%
  exclude_progress(min_progress = 98)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 5 out of 98 rows with incomplete progress were excluded, leaving 93 rows.

# Do not print rows to console
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_progress(print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows with incomplete progress were excluded, leaving 92 rows.
```
