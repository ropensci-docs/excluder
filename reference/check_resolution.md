# Check screen resolution

The `check_resolution()` function subsets rows of data, retaining rows
that have unacceptable screen resolution. This can be used, for example,
to determine data collected via phones when desktop monitors are
required. The function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
check_resolution(
  x,
  res_min = 1000,
  width_min = 0,
  height_min = 0,
  id_col = "ResponseId",
  res_col = "Resolution",
  rename = TRUE,
  keep = FALSE,
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

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

The output is a data frame of the rows that have unacceptable screen
resolutions. This includes new columns for resolution width and height.
For a function that marks these rows, use
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md).
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
[`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md),
[`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md)

Other check functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md),
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md)

## Examples

``` r
# Check for survey previews
data(qualtrics_text)
check_resolution(qualtrics_text)
#> ℹ 3 out of 100 rows had screen resolutions less than 0 or height less than 0.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-17 15:44:30 2020-12-17 15:47:57 IP Address   48.17.71.0      100
#> 2 2020-12-17 15:48:36 2020-12-17 15:52:01 IP Address 12.247.210.0      100
#> 3 2020-12-17 15:49:09 2020-12-17 15:52:41 IP Address 104.55.125.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   339     TRUE 2020-12-17 15:47:58 R_hEJfTQuUySzm9Ef
#> 2                   199     TRUE 2020-12-17 15:52:01 R_QdGXTRC6C6dg0xq
#> 3                   404     TRUE 2020-12-17 15:52:41 R_GWjtLqPIuuKgRZC
#>   LocationLatitude LocationLongitude UserLanguage       Browser       Version
#> 1         40.71187         -74.00837           EN Chrome iPhone  87.0.4280.77
#> 2         35.92692         -88.76387           EN        Chrome 87.0.4280.101
#> 3         41.44980         -82.18540           EN        Chrome 87.0.4280.101
#>   Operating System Resolution
#> 1           iPhone    375x667
#> 2       Android 10    360x740
#> 3        Android 9    360x740

# Remove preview data first
qualtrics_text %>%
  exclude_preview() %>%
  check_resolution()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 3 out of 98 rows had screen resolutions less than 0 or height less than 0.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-17 15:44:30 2020-12-17 15:47:57 IP Address   48.17.71.0      100
#> 2 2020-12-17 15:48:36 2020-12-17 15:52:01 IP Address 12.247.210.0      100
#> 3 2020-12-17 15:49:09 2020-12-17 15:52:41 IP Address 104.55.125.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   339     TRUE 2020-12-17 15:47:58 R_hEJfTQuUySzm9Ef
#> 2                   199     TRUE 2020-12-17 15:52:01 R_QdGXTRC6C6dg0xq
#> 3                   404     TRUE 2020-12-17 15:52:41 R_GWjtLqPIuuKgRZC
#>   LocationLatitude LocationLongitude UserLanguage       Browser       Version
#> 1         40.71187         -74.00837           EN Chrome iPhone  87.0.4280.77
#> 2         35.92692         -88.76387           EN        Chrome 87.0.4280.101
#> 3         41.44980         -82.18540           EN        Chrome 87.0.4280.101
#>   Operating System Resolution
#> 1           iPhone    375x667
#> 2       Android 10    360x740
#> 3        Android 9    360x740

# Do not print rows to console
qualtrics_text %>%
  exclude_preview() %>%
  check_resolution(print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 3 out of 98 rows had screen resolutions less than 0 or height less than 0.

# Do not print message to console
qualtrics_text %>%
  exclude_preview() %>%
  check_resolution(quiet = TRUE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-17 15:44:30 2020-12-17 15:47:57 IP Address   48.17.71.0      100
#> 2 2020-12-17 15:48:36 2020-12-17 15:52:01 IP Address 12.247.210.0      100
#> 3 2020-12-17 15:49:09 2020-12-17 15:52:41 IP Address 104.55.125.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   339     TRUE 2020-12-17 15:47:58 R_hEJfTQuUySzm9Ef
#> 2                   199     TRUE 2020-12-17 15:52:01 R_QdGXTRC6C6dg0xq
#> 3                   404     TRUE 2020-12-17 15:52:41 R_GWjtLqPIuuKgRZC
#>   LocationLatitude LocationLongitude UserLanguage       Browser       Version
#> 1         40.71187         -74.00837           EN Chrome iPhone  87.0.4280.77
#> 2         35.92692         -88.76387           EN        Chrome 87.0.4280.101
#> 3         41.44980         -82.18540           EN        Chrome 87.0.4280.101
#>   Operating System Resolution
#> 1           iPhone    375x667
#> 2       Android 10    360x740
#> 3        Android 9    360x740
```
