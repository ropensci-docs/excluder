# Check for duplicate IP addresses and/or locations

The `check_duplicates()` function subsets rows of data, retaining rows
that have the same IP address and/or same latitude and longitude. The
function is written to work with data from
[Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
check_duplicates(
  x,
  id_col = "ResponseId",
  ip_col = "IPAddress",
  location_col = c("LocationLatitude", "LocationLongitude"),
  rename = TRUE,
  dupl_ip = TRUE,
  dupl_location = TRUE,
  include_na = FALSE,
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

- ip_col:

  Column name for IP addresses.

- location_col:

  Two element vector specifying columns for latitude and longitude (in
  that order).

- rename:

  Logical indicating whether to rename columns (using
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md))

- dupl_ip:

  Logical indicating whether to check IP addresses.

- dupl_location:

  Logical indicating whether to check latitude and longitude.

- include_na:

  Logical indicating whether to include rows with NAs for IP address and
  location as potentially excluded rows.

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

An object of the same type as `x` that includes the rows with duplicate
IP addresses and/or locations. This includes a column called dupe_count
that returns the number of duplicates. For a function that marks these
rows, use
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md).
For a function that excludes these rows, use
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md).

## Details

To record this information in your Qualtrics survey, you must ensure
that [Anonymize responses is
disabled](https://www.qualtrics.com/support/survey-platform/survey-module/survey-options/survey-protection/#AnonymizingResponses).

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
By default, IP address and location are both checked, but they can be
checked separately with the `dupl_ip` and `dupl_location` arguments.

The function outputs to console separate messages about the number of
rows with duplicate IP addresses and rows with duplicate locations.
These counts are computed independently, so rows may be counted for both
types of duplicates.

## See also

Other duplicates functions:
[`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md),
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md)

Other check functions:
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md),
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md),
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)

## Examples

``` r
# Check for duplicate IP addresses and locations
data(qualtrics_text)
check_duplicates(qualtrics_text)
#> ℹ 2 NAs were found in IP addresses.
#> ℹ 7 out of 7 rows had duplicate IP addresses.
#> ℹ 1 NA was found in location.
#> ℹ 10 out of 10 rows had duplicate locations.
#>              StartDate             EndDate     Status    IPAddress Progress
#> 1  2020-12-11 12:41:23 2020-12-11 12:44:37 IP Address  24.195.91.0      100
#> 2  2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address   22.51.31.0       99
#> 3  2020-12-17 15:40:52 2020-12-17 15:43:39 IP Address 32.164.134.0      100
#> 4  2020-12-17 15:41:17 2020-12-17 15:45:42 IP Address  24.195.91.0      100
#> 5  2020-12-17 15:42:47 2020-12-17 15:46:26 IP Address  55.73.114.0      100
#> 6  2020-12-17 15:42:18 2020-12-17 15:48:00 IP Address  55.73.114.0      100
#> 7  2020-12-17 15:40:57 2020-12-17 15:48:56 IP Address   6.79.107.0      100
#> 8  2020-12-17 15:46:51 2020-12-17 15:51:38 IP Address   22.51.31.0      100
#> 9  2020-12-17 15:48:53 2020-12-17 15:53:48 IP Address   22.51.31.0      100
#> 10 2020-12-17 15:48:48 2020-12-17 15:54:12 IP Address 54.232.129.0      100
#>    Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                    177     TRUE 2020-12-11 12:44:37 R_LAt58JGEyKNWZlB
#> 2                    879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 3                    375     TRUE 2020-12-17 15:43:39 R_H5MqcQoWznreNBt
#> 4                    521     TRUE 2020-12-17 15:45:42 R_GNVaLC9Sb2ZDzQP
#> 5                    236     TRUE 2020-12-17 15:46:26 R_7UzegytocfkyrWC
#> 6                    526     TRUE 2020-12-17 15:48:00 R_NiK6d3RgjuJh1OI
#> 7                    397     TRUE 2020-12-17 15:48:56 R_8ezIj0X0p2lJuCQ
#> 8                    872     TRUE 2020-12-17 15:51:39 R_Gbz5en48KgnCXT7
#> 9                    246     TRUE 2020-12-17 15:53:48 R_AJfrQqClQNvWIch
#> 10                   149     TRUE 2020-12-17 15:54:12 R_Kc9BGXO793zEqHM
#>    LocationLatitude LocationLongitude UserLanguage Browser       Version
#> 1          40.33554         -75.92698           EN  Chrome 86.0.4240.198
#> 2          37.28265        -120.50248           EN  Chrome  87.0.4280.88
#> 3          45.50412        -122.78665           EN  Chrome  87.0.4280.88
#> 4          40.33554         -75.92698           EN  Chrome  87.0.4280.88
#> 5          28.56411         -81.54902           EN  Chrome  87.0.4280.88
#> 6          28.56411         -81.54902           EN  Chrome  87.0.4280.88
#> 7          45.50412        -122.78665           EN  Chrome  87.0.4280.88
#> 8          37.28265        -120.50248           EN Firefox          83.0
#> 9          37.28265        -120.50248           EN    Edge   84.0.522.52
#> 10         45.50412        -122.78665           EN  Chrome  87.0.4280.88
#>    Operating System Resolution
#> 1         Macintosh   1280x800
#> 2   Windows NT 10.0   1366x768
#> 3   Windows NT 10.0  1920x1080
#> 4    Windows NT 6.1   1366x768
#> 5   Windows NT 10.0  1920x1080
#> 6   Windows NT 10.0   1536x864
#> 7   Windows NT 10.0   1536x864
#> 8   Windows NT 10.0   1440x960
#> 9   Windows NT 10.0  1920x1080
#> 10  Windows NT 10.0  1920x1080

# Check only for duplicate locations
qualtrics_text %>%
  check_duplicates(dupl_location = FALSE)
#> ℹ 2 NAs were found in IP addresses.
#> ℹ 7 out of 7 rows had duplicate IP addresses.
#>             StartDate             EndDate     Status   IPAddress Progress
#> 1 2020-12-11 12:41:23 2020-12-11 12:44:37 IP Address 24.195.91.0      100
#> 2 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address  22.51.31.0       99
#> 3 2020-12-17 15:41:17 2020-12-17 15:45:42 IP Address 24.195.91.0      100
#> 4 2020-12-17 15:42:47 2020-12-17 15:46:26 IP Address 55.73.114.0      100
#> 5 2020-12-17 15:42:18 2020-12-17 15:48:00 IP Address 55.73.114.0      100
#> 6 2020-12-17 15:46:51 2020-12-17 15:51:38 IP Address  22.51.31.0      100
#> 7 2020-12-17 15:48:53 2020-12-17 15:53:48 IP Address  22.51.31.0      100
#>   Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                   177     TRUE 2020-12-11 12:44:37 R_LAt58JGEyKNWZlB
#> 2                   879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 3                   521     TRUE 2020-12-17 15:45:42 R_GNVaLC9Sb2ZDzQP
#> 4                   236     TRUE 2020-12-17 15:46:26 R_7UzegytocfkyrWC
#> 5                   526     TRUE 2020-12-17 15:48:00 R_NiK6d3RgjuJh1OI
#> 6                   872     TRUE 2020-12-17 15:51:39 R_Gbz5en48KgnCXT7
#> 7                   246     TRUE 2020-12-17 15:53:48 R_AJfrQqClQNvWIch
#>   LocationLatitude LocationLongitude UserLanguage Browser       Version
#> 1         40.33554         -75.92698           EN  Chrome 86.0.4240.198
#> 2         37.28265        -120.50248           EN  Chrome  87.0.4280.88
#> 3         40.33554         -75.92698           EN  Chrome  87.0.4280.88
#> 4         28.56411         -81.54902           EN  Chrome  87.0.4280.88
#> 5         28.56411         -81.54902           EN  Chrome  87.0.4280.88
#> 6         37.28265        -120.50248           EN Firefox          83.0
#> 7         37.28265        -120.50248           EN    Edge   84.0.522.52
#>   Operating System Resolution
#> 1        Macintosh   1280x800
#> 2  Windows NT 10.0   1366x768
#> 3   Windows NT 6.1   1366x768
#> 4  Windows NT 10.0  1920x1080
#> 5  Windows NT 10.0   1536x864
#> 6  Windows NT 10.0   1440x960
#> 7  Windows NT 10.0  1920x1080

# Do not print rows to console
qualtrics_text %>%
  check_duplicates(print = FALSE)
#> ℹ 2 NAs were found in IP addresses.
#> ℹ 7 out of 7 rows had duplicate IP addresses.
#> ℹ 1 NA was found in location.
#> ℹ 10 out of 10 rows had duplicate locations.

# Do not print message to console
qualtrics_text %>%
  check_duplicates(quiet = TRUE)
#>              StartDate             EndDate     Status    IPAddress Progress
#> 1  2020-12-11 12:41:23 2020-12-11 12:44:37 IP Address  24.195.91.0      100
#> 2  2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address   22.51.31.0       99
#> 3  2020-12-17 15:40:52 2020-12-17 15:43:39 IP Address 32.164.134.0      100
#> 4  2020-12-17 15:41:17 2020-12-17 15:45:42 IP Address  24.195.91.0      100
#> 5  2020-12-17 15:42:47 2020-12-17 15:46:26 IP Address  55.73.114.0      100
#> 6  2020-12-17 15:42:18 2020-12-17 15:48:00 IP Address  55.73.114.0      100
#> 7  2020-12-17 15:40:57 2020-12-17 15:48:56 IP Address   6.79.107.0      100
#> 8  2020-12-17 15:46:51 2020-12-17 15:51:38 IP Address   22.51.31.0      100
#> 9  2020-12-17 15:48:53 2020-12-17 15:53:48 IP Address   22.51.31.0      100
#> 10 2020-12-17 15:48:48 2020-12-17 15:54:12 IP Address 54.232.129.0      100
#>    Duration (in seconds) Finished        RecordedDate        ResponseId
#> 1                    177     TRUE 2020-12-11 12:44:37 R_LAt58JGEyKNWZlB
#> 2                    879    FALSE 2020-12-17 15:43:25 R_AkQyJypPyjgribz
#> 3                    375     TRUE 2020-12-17 15:43:39 R_H5MqcQoWznreNBt
#> 4                    521     TRUE 2020-12-17 15:45:42 R_GNVaLC9Sb2ZDzQP
#> 5                    236     TRUE 2020-12-17 15:46:26 R_7UzegytocfkyrWC
#> 6                    526     TRUE 2020-12-17 15:48:00 R_NiK6d3RgjuJh1OI
#> 7                    397     TRUE 2020-12-17 15:48:56 R_8ezIj0X0p2lJuCQ
#> 8                    872     TRUE 2020-12-17 15:51:39 R_Gbz5en48KgnCXT7
#> 9                    246     TRUE 2020-12-17 15:53:48 R_AJfrQqClQNvWIch
#> 10                   149     TRUE 2020-12-17 15:54:12 R_Kc9BGXO793zEqHM
#>    LocationLatitude LocationLongitude UserLanguage Browser       Version
#> 1          40.33554         -75.92698           EN  Chrome 86.0.4240.198
#> 2          37.28265        -120.50248           EN  Chrome  87.0.4280.88
#> 3          45.50412        -122.78665           EN  Chrome  87.0.4280.88
#> 4          40.33554         -75.92698           EN  Chrome  87.0.4280.88
#> 5          28.56411         -81.54902           EN  Chrome  87.0.4280.88
#> 6          28.56411         -81.54902           EN  Chrome  87.0.4280.88
#> 7          45.50412        -122.78665           EN  Chrome  87.0.4280.88
#> 8          37.28265        -120.50248           EN Firefox          83.0
#> 9          37.28265        -120.50248           EN    Edge   84.0.522.52
#> 10         45.50412        -122.78665           EN  Chrome  87.0.4280.88
#>    Operating System Resolution
#> 1         Macintosh   1280x800
#> 2   Windows NT 10.0   1366x768
#> 3   Windows NT 10.0  1920x1080
#> 4    Windows NT 6.1   1366x768
#> 5   Windows NT 10.0  1920x1080
#> 6   Windows NT 10.0   1536x864
#> 7   Windows NT 10.0   1536x864
#> 8   Windows NT 10.0   1440x960
#> 9   Windows NT 10.0  1920x1080
#> 10  Windows NT 10.0  1920x1080
```
