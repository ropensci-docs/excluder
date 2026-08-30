# Exclude survey previews

The `exclude_preview()` function removes rows that are survey previews.
The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
exclude_preview(
  x,
  id_col = "ResponseId",
  preview_col = "Status",
  rename = TRUE,
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

- preview_col:

  Column name for survey preview.

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

An object of the same type as `x` that excludes rows that are survey
previews. For a function that checks for these rows, use
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md).
For a function that marks these rows, use
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md).

## Details

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
The preview column in Qualtrics can be a numeric or character vector
depending on whether it is exported as choice text or numeric values.
This function works for both.

The function outputs to console a message about the number of rows that
are survey previews.

## See also

Other preview functions:
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md)

Other exclude functions:
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md),
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md),
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md),
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md),
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)

## Examples

``` r
# Exclude survey previews
data(qualtrics_text)
df <- exclude_preview(qualtrics_text)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.

# Works for Qualtrics data exported as numeric values, too
df <- qualtrics_numeric %>%
  exclude_preview()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.

# Do not print rows to console
df <- qualtrics_text %>%
  exclude_preview(print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
```
