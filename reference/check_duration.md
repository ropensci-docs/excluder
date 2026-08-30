# Check for minimum or maximum durations

The `check_duration()` function subsets rows of data, retaining rows
that have durations that are too fast or too slow. The function is
written to work with data from [Qualtrics](https://www.qualtrics.com/)
surveys.

## Usage

``` r
check_duration(
  x,
  min_duration = 10,
  max_duration = NULL,
  id_col = "ResponseId",
  duration_col = "Duration (in seconds)",
  rename = TRUE,
  keep = FALSE,
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

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

An object of the same type as `x` that includes the rows with fast
and/or slow duration. For a function that marks these rows, use
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md).
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
[`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md),
[`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md)

Other check functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md),
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md),
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)

## Examples

``` r
# Check for durations faster than 100 seconds
data(qualtrics_text)
check_duration(qualtrics_text, min_duration = 100)
#> ℹ 4 out of 100 rows took less time than 100.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-11 16:59:08 2020-12-11 17:02:05 IP Address  84.56.189.0      100
#> 2 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address   15.223.0.0       13
#> 3 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address  19.127.87.0       48
#> 4 2020-12-17 15:46:46 2020-12-17 15:49:02 IP Address 21.134.217.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                    54     TRUE 2020-12-11 17:02:05 R_2RQ5kfCKKHudpj3
#> 2                    40    FALSE 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 3                    74    FALSE 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 4                    72     TRUE 2020-12-17 15:49:02 R_PKKUJ04DtpTEire
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         43.23353         -77.55959           EN  Chrome 87.0.4280.88
#> 2         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 3         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 4         34.76243         -96.69044           EN  Chrome 87.0.4280.88
#>   Operating System Resolution
#> 1  Windows NT 10.0   1600x900
#> 2  Windows NT 10.0   1366x768
#> 3  Windows NT 10.0   1188x668
#> 4  Windows NT 10.0   1368x912

# Remove preview data first
qualtrics_text %>%
  exclude_preview() %>%
  check_duration(min_duration = 100)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 4 out of 98 rows took less time than 100.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-11 16:59:08 2020-12-11 17:02:05 IP Address  84.56.189.0      100
#> 2 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address   15.223.0.0       13
#> 3 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address  19.127.87.0       48
#> 4 2020-12-17 15:46:46 2020-12-17 15:49:02 IP Address 21.134.217.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                    54     TRUE 2020-12-11 17:02:05 R_2RQ5kfCKKHudpj3
#> 2                    40    FALSE 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 3                    74    FALSE 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 4                    72     TRUE 2020-12-17 15:49:02 R_PKKUJ04DtpTEire
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         43.23353         -77.55959           EN  Chrome 87.0.4280.88
#> 2         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 3         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 4         34.76243         -96.69044           EN  Chrome 87.0.4280.88
#>   Operating System Resolution
#> 1  Windows NT 10.0   1600x900
#> 2  Windows NT 10.0   1366x768
#> 3  Windows NT 10.0   1188x668
#> 4  Windows NT 10.0   1368x912

# Check only for durations slower than 800 seconds
qualtrics_text %>%
  exclude_preview() %>%
  check_duration(max_duration = 800)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 0 out of 98 rows took less time than 10.
#> ℹ 2 out of 98 rows took more time than 800.
#>             StartDate             EndDate     Status  IPAddress Progress
#> 1 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address 22.51.31.0       99
#> 2 2020-12-17 15:46:51 2020-12-17 15:51:38 IP Address 22.51.31.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 2                   872     TRUE 2020-12-17 15:51:39 R_Gbz5en48KgnCXT7
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         37.28265         -120.5025           EN  Chrome 87.0.4280.88
#> 2         37.28265         -120.5025           EN Firefox         83.0
#>   Operating System Resolution
#> 1  Windows NT 10.0   1366x768
#> 2  Windows NT 10.0   1440x960

# Do not print rows to console
qualtrics_text %>%
  exclude_preview() %>%
  check_duration(min_duration = 100, print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 4 out of 98 rows took less time than 100.

# Do not print message to console
qualtrics_text %>%
  exclude_preview() %>%
  check_duration(min_duration = 100, quiet = TRUE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#>             StartDate             EndDate     Status    IPAddress Progress
#> 1 2020-12-11 16:59:08 2020-12-11 17:02:05 IP Address  84.56.189.0      100
#> 2 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address   15.223.0.0       13
#> 3 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address  19.127.87.0       48
#> 4 2020-12-17 15:46:46 2020-12-17 15:49:02 IP Address 21.134.217.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                    54     TRUE 2020-12-11 17:02:05 R_2RQ5kfCKKHudpj3
#> 2                    40    FALSE 2020-12-17 15:46:37 R_Dx6w74UfhnGhAmj
#> 3                    74    FALSE 2020-12-17 15:46:46 R_ewyyOOPADLGo9xZ
#> 4                    72     TRUE 2020-12-17 15:49:02 R_PKKUJ04DtpTEire
#>   LocationLatitude LocationLongitude UserLanguage Browser      Version
#> 1         43.23353         -77.55959           EN  Chrome 87.0.4280.88
#> 2         34.77804         -84.96198           DE  Chrome 83.0.4103.61
#> 3         33.66715        -117.82543           EN  Chrome 87.0.4280.88
#> 4         34.76243         -96.69044           EN  Chrome 87.0.4280.88
#>   Operating System Resolution
#> 1  Windows NT 10.0   1600x900
#> 2  Windows NT 10.0   1366x768
#> 3  Windows NT 10.0   1188x668
#> 4  Windows NT 10.0   1368x912
```
