# Check for survey progress

The `check_progress()` function subsets rows of data, retaining rows
that have incomplete progress. The function is written to work with data
from [Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
check_progress(
  x,
  min_progress = 100,
  id_col = "ResponseId",
  finished_col = "Finished",
  progress_col = "Progress",
  rename = TRUE,
  keep = FALSE,
  quiet = FALSE,
  print = TRUE
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

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

The output is a data frame of the rows that have incomplete progress.
For a function that marks these rows, use
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md).
For a function that excludes these rows, use
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md).

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
[`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md),
[`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md)

Other check functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md),
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)

## Examples

``` r
# Check for rows with incomplete progress
data(qualtrics_text)
check_progress(qualtrics_text)
#> ℹ 6 out of 100 rows did not complete the study.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address   22.51.31.0       99
#> 2 2020-12-17 15:40:56 2020-12-17 15:46:23 IP Address 71.146.112.0        1
#> 3 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address   15.223.0.0       13
#> 4 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address  19.127.87.0       48
#> 5 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#> 6 2020-12-17 15:49:28 2020-12-17 15:55:06 IP Address   2.246.67.0       44
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 2                   627    FALSE 2020-12-17 15:46:23 R_1cvaTAoXO7XW1vi
#> 3                    40    FALSE 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 4                    74    FALSE 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 5                   307    FALSE 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#> 6                   355    FALSE 2020-12-17 15:55:06 R_UfSQq1VXYkVcxdJ
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         37.28265        -120.50248           EN  Chrome 87.0.4280.88
#> 2         34.03605        -117.04066           EN    <NA>         <NA>
#> 3         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 4         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 5         29.29882         -81.04289           EN Firefox         81.0
#> 6         37.79058        -121.96737           EN  Chrome 87.0.4280.67
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2             <NA>       <NA>
#> 3  Windows NT 10.0   1366x768
#> 4  Windows NT 10.0   1188x668
#> 5  Windows NT 10.0   1366x768
#> 6        Macintosh   1280x800

# Remove preview data first
qualtrics_text %>%
  exclude_preview() %>%
  check_progress()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows did not complete the study.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address   22.51.31.0       99
#> 2 2020-12-17 15:40:56 2020-12-17 15:46:23 IP Address 71.146.112.0        1
#> 3 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address   15.223.0.0       13
#> 4 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address  19.127.87.0       48
#> 5 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#> 6 2020-12-17 15:49:28 2020-12-17 15:55:06 IP Address   2.246.67.0       44
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 2                   627    FALSE 2020-12-17 15:46:23 R_1cvaTAoXO7XW1vi
#> 3                    40    FALSE 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 4                    74    FALSE 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 5                   307    FALSE 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#> 6                   355    FALSE 2020-12-17 15:55:06 R_UfSQq1VXYkVcxdJ
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         37.28265        -120.50248           EN  Chrome 87.0.4280.88
#> 2         34.03605        -117.04066           EN    <NA>         <NA>
#> 3         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 4         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 5         29.29882         -81.04289           EN Firefox         81.0
#> 6         37.79058        -121.96737           EN  Chrome 87.0.4280.67
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2             <NA>       <NA>
#> 3  Windows NT 10.0   1366x768
#> 4  Windows NT 10.0   1188x668
#> 5  Windows NT 10.0   1366x768
#> 6        Macintosh   1280x800

# Include a lower acceptable completion percentage
qualtrics_numeric %>%
  exclude_preview() %>%
  check_progress(min_progress = 98)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 rows did not complete the study, and 5 of those completed less than 98% of the study.
#>             StartDate             EndDate Status    IPAddress Progress
#> 1 2020-12-17 15:40:56 2020-12-17 15:46:23      0 71.146.112.0        1
#> 2 2020-12-17 15:41:52 2020-12-17 15:46:37      0   15.223.0.0       13
#> 3 2020-12-17 15:41:27 2020-12-17 15:46:45      0  19.127.87.0       48
#> 4 2020-12-17 15:49:42 2020-12-17 15:51:50      0 40.146.247.0        5
#> 5 2020-12-17 15:49:28 2020-12-17 15:55:06      0   2.246.67.0       44
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   627        0 2020-12-17 15:46:23 R_1cvaTAoXO7XW1vi
#> 2                    40        0 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 3                    74        0 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 4                   307        0 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#> 5                   355        0 2020-12-17 15:55:06 R_UfSQq1VXYkVcxdJ
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         34.03605        -117.04066           EN    <NA>         <NA>
#> 2         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 3         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 4         29.29882         -81.04289           EN Firefox         81.0
#> 5         37.79058        -121.96737           EN  Chrome 87.0.4280.67
#>   Operating System Resolution
#> 1             <NA>       <NA>
#> 2  Windows NT 10.0   1366x768
#> 3  Windows NT 10.0   1188x668
#> 4  Windows NT 10.0   1366x768
#> 5        Macintosh   1280x800

# Do not print rows to console
qualtrics_text %>%
  exclude_preview() %>%
  check_progress(print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows did not complete the study.

# Do not print message to console
qualtrics_text %>%
  exclude_preview() %>%
  check_progress(quiet = TRUE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address   22.51.31.0       99
#> 2 2020-12-17 15:40:56 2020-12-17 15:46:23 IP Address 71.146.112.0        1
#> 3 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address   15.223.0.0       13
#> 4 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address  19.127.87.0       48
#> 5 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#> 6 2020-12-17 15:49:28 2020-12-17 15:55:06 IP Address   2.246.67.0       44
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 2                   627    FALSE 2020-12-17 15:46:23 R_1cvaTAoXO7XW1vi
#> 3                    40    FALSE 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 4                    74    FALSE 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 5                   307    FALSE 2020-12-17 15:51:50 R_HFKclPO5wWGNsFs
#> 6                   355    FALSE 2020-12-17 15:55:06 R_UfSQq1VXYkVcxdJ
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         37.28265        -120.50248           EN  Chrome 87.0.4280.88
#> 2         34.03605        -117.04066           EN    <NA>         <NA>
#> 3         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 4         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 5         29.29882         -81.04289           EN Firefox         81.0
#> 6         37.79058        -121.96737           EN  Chrome 87.0.4280.67
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2             <NA>       <NA>
#> 3  Windows NT 10.0   1366x768
#> 4  Windows NT 10.0   1188x668
#> 5  Windows NT 10.0   1366x768
#> 6        Macintosh   1280x800
```
