# Mark unacceptable screen resolution

The `mark_resolution()` function creates a column labeling rows that
have unacceptable screen resolution. The function is written to work
with data from [Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
mark_resolution(
  x,
  res_min = 1000,
  width_min = 0,
  height_min = 0,
  id_col = "ResponseId",
  res_col = "Resolution",
  rename = TRUE,
  quiet = FALSE,
  print = TRUE
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

## Value

An object of the same type as `x` that includes a column marking rows
that have unacceptable screen resolutions. For a function that checks
for these rows, use
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md).
For a function that excludes these rows, use
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md).

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
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)

Other mark functions:
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md),
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md),
[`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md),
[`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md),
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md),
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md)

## Examples

``` r
# Mark low screen resolutions
data(qualtrics_text)
df <- mark_resolution(qualtrics_text)
#> ℹ 3 out of 100 rows had screen resolutions less than 0 or height less than 0.

# Remove preview data first
df <- qualtrics_text %>%
  exclude_preview() %>%
  mark_resolution()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 3 out of 98 rows had screen resolutions less than 0 or height less than 0.
```
