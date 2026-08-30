# Check for survey previews

The `check_preview()` function subsets rows of data, retaining rows that
are survey previews. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
check_preview(
  x,
  id_col = "ResponseId",
  preview_col = "Status",
  rename = TRUE,
  keep = FALSE,
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

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

The output is a data frame of the rows that are survey previews. For a
function that marks these rows, use
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md).
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
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md),
[`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md)

Other check functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md),
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md),
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)

## Examples

``` r
# Check for survey previews
data(qualtrics_text)
check_preview(qualtrics_text)
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#>             StartDate             EndDate         Status IPAddress Progress
#> 1 2020-12-11 12:06:52 2020-12-11 12:10:30 Survey Preview      <NA>      100
#> 2 2020-12-11 12:06:43 2020-12-11 12:11:27 Survey Preview      <NA>      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   465     TRUE 2020-12-11 12:10:30 R_xLWiuPaNuURSFXY
#> 2                   545     TRUE 2020-12-11 12:11:27 R_Q5lqYw6emJQZx2o
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         29.73694         -94.97599           EN  Chrome 88.0.4324.41
#> 2         39.74107        -121.82490           EN  Chrome 88.0.4324.50
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2        Macintosh  1680x1050

# Works for Qualtrics data exported as numeric values, too
qualtrics_numeric %>%
  check_preview()
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#>             StartDate             EndDate Status IPAddress Progress
#> 1 2020-12-11 12:06:52 2020-12-11 12:10:30      1      <NA>      100
#> 2 2020-12-11 12:06:43 2020-12-11 12:11:27      1      <NA>      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   465        1 2020-12-11 12:10:30 R_xLWiuPaNuURSFXY
#> 2                   545        1 2020-12-11 12:11:27 R_Q5lqYw6emJQZx2o
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         29.73694         -94.97599           EN  Chrome 88.0.4324.41
#> 2         39.74107        -121.82490           EN  Chrome 88.0.4324.50
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2        Macintosh  1680x1050

# Do not print rows to console
qualtrics_text %>%
  check_preview(print = FALSE)
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.

# Do not print message to console
qualtrics_text %>%
  check_preview(quiet = TRUE)
#>             StartDate             EndDate         Status IPAddress Progress
#> 1 2020-12-11 12:06:52 2020-12-11 12:10:30 Survey Preview      <NA>      100
#> 2 2020-12-11 12:06:43 2020-12-11 12:11:27 Survey Preview      <NA>      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   465     TRUE 2020-12-11 12:10:30 R_xLWiuPaNuURSFXY
#> 2                   545     TRUE 2020-12-11 12:11:27 R_Q5lqYw6emJQZx2o
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         29.73694         -94.97599           EN  Chrome 88.0.4324.41
#> 2         39.74107        -121.82490           EN  Chrome 88.0.4324.50
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2        Macintosh  1680x1050
```
