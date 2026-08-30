# Exclude unacceptable screen resolution

The `exclude_resolution()` function removes rows that have unacceptable
screen resolution. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
exclude_resolution(
  x,
  res_min = 1000,
  width_min = 0,
  height_min = 0,
  id_col = "ResponseId",
  res_col = "Resolution",
  rename = TRUE,
  quiet = TRUE,
  print = TRUE,
  silent = FALSE
)
```

## Arguments

- x:

  Data frame (preferably imported from Qualtrics using {qualtRics}).

- res_min:

  Minimum acceptable screen resolution (width and height).

- width_min:

  Minimum acceptable screen width.

- height_min:

  Minimum acceptable screen height.

- id_col:

  Column name for unique row ID (e.g., participant).

- res_col:

  Column name for screen resolution (in format widthxheight).

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
unacceptable screen resolutions. For a function that checks for these
rows, use
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md).
For a function that marks these rows, use
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md).

## Details

To record this information in your Qualtrics survey, you must [insert a
meta info
question](https://www.qualtrics.com/support/survey-platform/survey-module/editing-questions/question-types-guide/advanced/meta-info-question/).

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).

The function outputs to console a message about the number of rows with
unacceptable screen resolution.

## See also

Other resolution functions:
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md),
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md)

Other exclude functions:
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md),
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md),
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md),
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md),
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md)

## Examples

``` r
# Exclude low screen resolutions
data(qualtrics_text)
df <- exclude_resolution(qualtrics_text)
#> ℹ 3 out of 100 rows with unacceptable screen resolution were excluded, leaving 97 rows.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  exclude_resolution()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 3 out of 98 rows with unacceptable screen resolution were excluded, leaving 95 rows.
```
