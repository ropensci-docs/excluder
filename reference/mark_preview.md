# Mark survey previews

The `mark_preview()` function creates a column labeling rows that are
survey previews. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
mark_preview(
  x,
  id_col = "ResponseId",
  preview_col = "Status",
  rename = TRUE,
  quiet = FALSE,
  print = TRUE
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

## Value

An object of the same type as `x` that includes a column marking rows
that are survey previews. For a function that checks for these rows, use
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md).
For a function that excludes these rows, use
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md).

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
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md)

Other mark functions:
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md),
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md),
[`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md),
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md),
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md),
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md)

## Examples

``` r
# Mark survey previews
data(qualtrics_text)
df <- mark_preview(qualtrics_text)
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.

# Works for Qualtrics data exported as numeric values, too
df <- qualtrics_numeric %>%
  mark_preview()
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
```
