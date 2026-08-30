# excluder

``` r

library(excluder)
```

The [excluder](https://docs.ropensci.org/excluder/) package facilitates
marking, checking for, and excluding rows of data frames[^1] for common
online participant exclusion criteria. This package applies to online
data with a focus on data collected from
[Qualtrics](https://www.qualtrics.com) surveys, and default column names
come from importing data with the
[`{qualtRics}`](https://docs.ropensci.org/qualtRics/) package. This may
be most useful for [Mechanical Turk](https://www.mturk.com/) data to
screen for duplicate entries from the same location/IP address or
entries from locations outside of the United States. However, the
package can be used for data from other sources that need to be screened
for IP address, location, duplicated locations, participant progress,
survey completion time, and/or screen resolution.

## Usage

The package has three core verbs:

1.  `mark_*()` functions add a new column to the original data frame
    that labels the rows meeting the exclusion criteria. This is useful
    to label the potential exclusions for future processing without
    changing the original data frame.
2.  `check_*()` functions search for the exclusion criteria and output a
    message with the number of rows meeting the criteria and a data
    frame of the rows meeting the criteria. This is useful for viewing
    the potential exclusions.
3.  `exclude_*()` functions remove rows meeting the exclusion criteria.
    This is safest to do after checking the rows to ensure the
    exclusions are correct.

The `check_*()` and `exclude_*()` functions call the `mark_*()` function
internally and then filter either the excluded or non-excluded rows. So
avoid combining different verbs to sidestep unnecessary passes through
the data.

The package provides seven types of exclusions based on Qualtrics
metadata:

1.  `duplicates` works with rows that have duplicate IP addresses and/or
    locations (latitude/longitude), using
    [`janitor::get_dupes()`](https://sfirke.github.io/janitor/reference/get_dupes.html).
2.  `duration` works with rows whose survey completion time is too short
    and/or too long.
3.  `ip` works with rows whose IP addresses are not found in the
    specified country (note: this exclusion type requires an internet
    connection to download the country’s IP ranges), using package
    [`{ipaddress}`](https://github.com/davidchall/ipaddress).
4.  `location` works with rows whose latitude and longitude are not
    found in the United States.
5.  `preview` works with rows that are survey previews.
6.  `progress` works with rows in which the survey was not complete.
7.  `resolution` works with rows whose screen resolution is not
    acceptable.

The verbs combine with the exclusion types to generate functions. For
instance,
[`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md)
will mark duplicate rows and
[`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md)
will exclude preview rows.

There are also helper functions:

1.  [`unite_exclusions()`](https://docs.ropensci.org//excluder/reference/unite_exclusions.html)
    unites all of the columns marked by `mark` functions into a single
    column (each use of a `mark` function creates a new column).
2.  [`deidentify()`](https://docs.ropensci.org//excluder/reference/deidentify.html)
    removes standard Qualtrics columns with identifiable information.
3.  [`remove_label_rows()`](https://docs.ropensci.org//excluder/reference/remove_label_rows.html)
    removes the first two rows of labels and convert date and numeric
    columns in the metadata.

## Preparing your data

If you use the
[`fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html)
from the [qualtRics](https://docs.ropensci.org/qualtRics/) package to
import your Qualtrics data, it will automatically remove the first two
label rows from the data set. However, if you directly download your
data from Qualtrics, it will include two rows in your data set that
include label information. This has two side effects: (1) there are
non-data rows that need to be removed from your data set, and (2) all of
your columns will be imported as character data types.

The
[`remove_label_rows()`](https://docs.ropensci.org//excluder/reference/remove_label_rows.html)
function will remove these two label rows. Also, by default, it will
coerce the Qualtrics metadata columns from character types to the
correct formats (e.g., *`StartDate`* is coerced to a date, *`Progress`*
is coerced to numeric). So if you download your data from Qualtrics, you
will need to run this function on your data before proceeding.

``` r

dplyr::glimpse(qualtrics_raw)
#> Rows: 102
#> Columns: 16
#> $ StartDate               <chr> "Start Date", "{\"ImportId\":\"startDate\",\"t…
#> $ EndDate                 <chr> "End Date", "{\"ImportId\":\"endDate\",\"timeZ…
#> $ Status                  <chr> "Response Type", "{\"ImportId\":\"status\"}", …
#> $ IPAddress               <chr> "IP Address", "{\"ImportId\":\"ipAddress\"}", …
#> $ Progress                <chr> "Progress", "{\"ImportId\":\"progress\"}", "10…
#> $ `Duration (in seconds)` <chr> "Duration (in seconds)", "{\"ImportId\":\"dura…
#> $ Finished                <chr> "Finished", "{\"ImportId\":\"finished\"}", "TR…
#> $ RecordedDate            <chr> "Recorded Date", "{\"ImportId\":\"recordedDate…
#> $ ResponseId              <chr> "Response ID", "{\"ImportId\":\"_recordId\"}",…
#> $ LocationLatitude        <chr> "Location Latitude", "{\"ImportId\":\"location…
#> $ LocationLongitude       <chr> "Location Longitude", "{\"ImportId\":\"locatio…
#> $ UserLanguage            <chr> "User Language", "{\"ImportId\":\"userLanguage…
#> $ Browser                 <chr> "Browser Meta Info - Browser", "{\"ImportId\":…
#> $ Version                 <chr> "Browser Meta Info - Version", "{\"ImportId\":…
#> $ `Operating System`      <chr> "Browser Meta Info - Operating System", "{\"Im…
#> $ Resolution              <chr> "Browser Meta Info - Resolution", "{\"ImportId…
#
# Remove label rows and coerce metadata columns
df <- remove_label_rows(qualtrics_raw) %>%
  dplyr::glimpse()
#> Rows: 100
#> Columns: 16
#> $ StartDate               <dttm> 2020-12-11 12:06:52, 2020-12-11 12:06:43, 202…
#> $ EndDate                 <dttm> 2020-12-11 12:10:30, 2020-12-11 12:11:27, 202…
#> $ Status                  <chr> "Survey Preview", "Survey Preview", "IP Addres…
#> $ IPAddress               <chr> NA, NA, "73.23.43.0", "16.140.105.0", "107.57.…
#> $ Progress                <dbl> 100, 100, 100, 100, 100, 100, 100, 100, 100, 1…
#> $ `Duration (in seconds)` <dbl> 465, 545, 651, 409, 140, 213, 177, 662, 296, 2…
#> $ Finished                <lgl> TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
#> $ RecordedDate            <dttm> 2020-12-11 12:10:30, 2020-12-11 12:11:27, 202…
#> $ ResponseId              <chr> "R_xLWiuPaNuURSFXY", "R_Q5lqYw6emJQZx2o", "R_f…
#> $ LocationLatitude        <dbl> 29.73694, 39.74107, 34.03852, 44.96581, 27.980…
#> $ LocationLongitude       <dbl> -94.97599, -121.82490, -118.25739, -93.07187, …
#> $ UserLanguage            <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN"…
#> $ Browser                 <chr> "Chrome", "Chrome", "Chrome", "Chrome", "Chrom…
#> $ Version                 <chr> "88.0.4324.41", "88.0.4324.50", "87.0.4280.88"…
#> $ `Operating System`      <chr> "Windows NT 10.0", "Macintosh", "Windows NT 10…
#> $ Resolution              <chr> "1366x768", "1680x1050", "1366x768", "1536x864…
```

## Marking observations

The core verbs in this package mark them for future processing. The
`mark_*()` suite of functions creates a new column for each mark
function used that marks which observations violate the exclusion
criterion. They print a message about the number of observations meeting
each exclusion criteria. Mark functions return a data frame identical to
the original with additional columns marking exclusions.

``` r

# Mark observations run as preview
df %>%
  mark_preview() %>%
  dplyr::glimpse()
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#> Rows: 100
#> Columns: 17
#> $ StartDate               <dttm> 2020-12-11 12:06:52, 2020-12-11 12:06:43, 202…
#> $ EndDate                 <dttm> 2020-12-11 12:10:30, 2020-12-11 12:11:27, 202…
#> $ Status                  <chr> "Survey Preview", "Survey Preview", "IP Addres…
#> $ IPAddress               <chr> NA, NA, "73.23.43.0", "16.140.105.0", "107.57.…
#> $ Progress                <dbl> 100, 100, 100, 100, 100, 100, 100, 100, 100, 1…
#> $ `Duration (in seconds)` <dbl> 465, 545, 651, 409, 140, 213, 177, 662, 296, 2…
#> $ Finished                <lgl> TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
#> $ RecordedDate            <dttm> 2020-12-11 12:10:30, 2020-12-11 12:11:27, 202…
#> $ ResponseId              <chr> "R_xLWiuPaNuURSFXY", "R_Q5lqYw6emJQZx2o", "R_f…
#> $ LocationLatitude        <dbl> 29.73694, 39.74107, 34.03852, 44.96581, 27.980…
#> $ LocationLongitude       <dbl> -94.97599, -121.82490, -118.25739, -93.07187, …
#> $ UserLanguage            <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN"…
#> $ Browser                 <chr> "Chrome", "Chrome", "Chrome", "Chrome", "Chrom…
#> $ Version                 <chr> "88.0.4324.41", "88.0.4324.50", "87.0.4280.88"…
#> $ `Operating System`      <chr> "Windows NT 10.0", "Macintosh", "Windows NT 10…
#> $ Resolution              <chr> "1366x768", "1680x1050", "1366x768", "1536x864…
#> $ exclusion_preview       <chr> "preview", "preview", "", "", "", "", "", "", …
```

Notice the new *`exclusion_preview`* column at the end of the data
frame. It has marked the first two observations as `preview`.

Piping multiple mark functions will create multiple rows marking
observations for exclusion.

``` r

# Mark preview and incomplete observations
df %>%
  mark_preview() %>%
  mark_progress() %>%
  dplyr::glimpse()
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#> ℹ 6 out of 100 rows did not complete the study.
#> Rows: 100
#> Columns: 18
#> $ StartDate               <dttm> 2020-12-11 12:06:52, 2020-12-11 12:06:43, 202…
#> $ EndDate                 <dttm> 2020-12-11 12:10:30, 2020-12-11 12:11:27, 202…
#> $ Status                  <chr> "Survey Preview", "Survey Preview", "IP Addres…
#> $ IPAddress               <chr> NA, NA, "73.23.43.0", "16.140.105.0", "107.57.…
#> $ Progress                <dbl> 100, 100, 100, 100, 100, 100, 100, 100, 100, 1…
#> $ `Duration (in seconds)` <dbl> 465, 545, 651, 409, 140, 213, 177, 662, 296, 2…
#> $ Finished                <lgl> TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
#> $ RecordedDate            <dttm> 2020-12-11 12:10:30, 2020-12-11 12:11:27, 202…
#> $ ResponseId              <chr> "R_xLWiuPaNuURSFXY", "R_Q5lqYw6emJQZx2o", "R_f…
#> $ LocationLatitude        <dbl> 29.73694, 39.74107, 34.03852, 44.96581, 27.980…
#> $ LocationLongitude       <dbl> -94.97599, -121.82490, -118.25739, -93.07187, …
#> $ UserLanguage            <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN"…
#> $ Browser                 <chr> "Chrome", "Chrome", "Chrome", "Chrome", "Chrom…
#> $ Version                 <chr> "88.0.4324.41", "88.0.4324.50", "87.0.4280.88"…
#> $ `Operating System`      <chr> "Windows NT 10.0", "Macintosh", "Windows NT 10…
#> $ Resolution              <chr> "1366x768", "1680x1050", "1366x768", "1536x864…
#> $ exclusion_preview       <chr> "preview", "preview", "", "", "", "", "", "", …
#> $ exclusion_progress      <chr> "", "", "", "", "", "", "", "", "", "", "", ""…
```

To unite all of the marked columns into a single column, use the
[`unite_exclusions()`](https://docs.ropensci.org/excluder/reference/unite_exclusions.md)
function. This will create a new `exclusions` columns that will unite
all exclusions in each observation into a single column. Here we move
the combined *`exclusions`* column to the beginning of the data frame to
view it.

``` r

df %>%
  mark_preview() %>%
  mark_duration(min = 500) %>%
  unite_exclusions() %>%
  dplyr::relocate(exclusions, .before = StartDate)
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#> ℹ 82 out of 100 rows took less time than 500.
#> # A tibble: 100 × 17
#>    exclusions  StartDate           EndDate             Status IPAddress Progress
#>    <chr>       <dttm>              <dttm>              <chr>  <chr>        <dbl>
#>  1 "preview,d… 2020-12-11 12:06:52 2020-12-11 12:10:30 Surve… NA             100
#>  2 "preview"   2020-12-11 12:06:43 2020-12-11 12:11:27 Surve… NA             100
#>  3 ""          2020-12-11 12:17:22 2020-12-11 12:21:41 IP Ad… 73.23.43…      100
#>  4 "duration_… 2020-12-11 12:17:41 2020-12-11 12:22:07 IP Ad… 16.140.1…      100
#>  5 "duration_… 2020-12-11 12:19:45 2020-12-11 12:23:16 IP Ad… 107.57.2…      100
#>  6 "duration_… 2020-12-11 12:37:51 2020-12-11 12:43:09 IP Ad… 15.232.1…      100
#>  7 "duration_… 2020-12-11 12:41:23 2020-12-11 12:44:37 IP Ad… 24.195.9…      100
#>  8 ""          2020-12-11 12:37:04 2020-12-11 12:45:50 IP Ad… 98.75.20…      100
#>  9 "duration_… 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Ad… 51.113.1…      100
#> 10 "duration_… 2020-12-11 13:22:34 2020-12-11 13:35:19 IP Ad… 17.163.1…      100
#> # ℹ 90 more rows
#> # ℹ 11 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>
```

Multiple exclusions are separated by `,` by default, but the separating
character can be controlled by the `separator` argument. By default, the
multiple exclusion columns are removed from the final data frame, but
this can be turned off by setting the `remove` argument to `FALSE`.

``` r

df %>%
  mark_preview() %>%
  mark_duration(min = 500) %>%
  unite_exclusions(separator = ";", remove = FALSE) %>%
  dplyr::relocate(exclusions, .before = StartDate)
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#> ℹ 82 out of 100 rows took less time than 500.
#> # A tibble: 100 × 19
#>    exclusions  StartDate           EndDate             Status IPAddress Progress
#>    <chr>       <dttm>              <dttm>              <chr>  <chr>        <dbl>
#>  1 "preview;d… 2020-12-11 12:06:52 2020-12-11 12:10:30 Surve… NA             100
#>  2 "preview"   2020-12-11 12:06:43 2020-12-11 12:11:27 Surve… NA             100
#>  3 ""          2020-12-11 12:17:22 2020-12-11 12:21:41 IP Ad… 73.23.43…      100
#>  4 "duration_… 2020-12-11 12:17:41 2020-12-11 12:22:07 IP Ad… 16.140.1…      100
#>  5 "duration_… 2020-12-11 12:19:45 2020-12-11 12:23:16 IP Ad… 107.57.2…      100
#>  6 "duration_… 2020-12-11 12:37:51 2020-12-11 12:43:09 IP Ad… 15.232.1…      100
#>  7 "duration_… 2020-12-11 12:41:23 2020-12-11 12:44:37 IP Ad… 24.195.9…      100
#>  8 ""          2020-12-11 12:37:04 2020-12-11 12:45:50 IP Ad… 98.75.20…      100
#>  9 "duration_… 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Ad… 51.113.1…      100
#> 10 "duration_… 2020-12-11 13:22:34 2020-12-11 13:35:19 IP Ad… 17.163.1…      100
#> # ℹ 90 more rows
#> # ℹ 13 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>, exclusion_preview <chr>,
#> #   exclusion_duration <chr>
```

## Checking observations

The `check_*()` suite of functions return a data frame that includes
only the observations that meet the criterion. Since these functions
first run the appropriate `mark_*()` function, they also print a message
about the number of observations that meet the exclusion criterion.

``` r

# Check for rows with incomplete data
df %>%
  check_progress()
#> ℹ 6 out of 100 rows did not complete the study.
#> # A tibble: 6 × 16
#>   StartDate           EndDate             Status     IPAddress    Progress
#>   <dttm>              <dttm>              <chr>      <chr>           <dbl>
#> 1 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address 22.51.31.0         99
#> 2 2020-12-17 15:40:56 2020-12-17 15:46:23 IP Address 71.146.112.0        1
#> 3 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address 15.223.0.0         13
#> 4 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address 19.127.87.0        48
#> 5 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#> 6 2020-12-17 15:49:28 2020-12-17 15:55:06 IP Address 2.246.67.0         44
#> # ℹ 11 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>
```

``` r

# Check for rows with durations less than 100 seconds
df %>%
  check_duration(min_duration = 100)
#> ℹ 4 out of 100 rows took less time than 100.
#> # A tibble: 4 × 16
#>   StartDate           EndDate             Status     IPAddress    Progress
#>   <dttm>              <dttm>              <chr>      <chr>           <dbl>
#> 1 2020-12-11 16:59:08 2020-12-11 17:02:05 IP Address 84.56.189.0       100
#> 2 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address 15.223.0.0         13
#> 3 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address 19.127.87.0        48
#> 4 2020-12-17 15:46:46 2020-12-17 15:49:02 IP Address 21.134.217.0      100
#> # ℹ 11 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>
```

Because checks return only the rows meeting the criteria, they **should
not be connected via pipes** unless you want to subset the second check
criterion within the rows that meet the first criterion.

``` r

# Check for rows with durations less than 100 seconds in rows that did not complete the survey
df %>%
  check_progress() %>%
  check_duration(min_duration = 100)
#> ℹ 6 out of 100 rows did not complete the study.
#> ℹ 2 out of 6 rows took less time than 100.
#> # A tibble: 2 × 16
#>   StartDate           EndDate             Status     IPAddress   Progress
#>   <dttm>              <dttm>              <chr>      <chr>          <dbl>
#> 1 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address 15.223.0.0        13
#> 2 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address 19.127.87.0       48
#> # ℹ 11 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>
```

To check all data for multiple criteria, use the `mark_*()` functions
followed by a filter.

``` r

# Check for multiple criteria
df %>%
  mark_preview() %>%
  mark_duration(min = 500) %>%
  unite_exclusions() %>%
  dplyr::filter(exclusions != "")
#> ℹ 2 rows were collected as previews. It is highly recommended to exclude these rows before further processing.
#> ℹ 82 out of 100 rows took less time than 500.
#> # A tibble: 83 × 17
#>    StartDate           EndDate             Status         IPAddress    Progress
#>    <dttm>              <dttm>              <chr>          <chr>           <dbl>
#>  1 2020-12-11 12:06:52 2020-12-11 12:10:30 Survey Preview NA                100
#>  2 2020-12-11 12:06:43 2020-12-11 12:11:27 Survey Preview NA                100
#>  3 2020-12-11 12:17:41 2020-12-11 12:22:07 IP Address     16.140.105.0      100
#>  4 2020-12-11 12:19:45 2020-12-11 12:23:16 IP Address     107.57.244.0      100
#>  5 2020-12-11 12:37:51 2020-12-11 12:43:09 IP Address     15.232.167.0      100
#>  6 2020-12-11 12:41:23 2020-12-11 12:44:37 IP Address     24.195.91.0       100
#>  7 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Address     51.113.171.0      100
#>  8 2020-12-11 13:22:34 2020-12-11 13:35:19 IP Address     17.163.199.0      100
#>  9 2020-12-11 16:59:08 2020-12-11 17:02:05 IP Address     84.56.189.0       100
#> 10 2020-12-11 17:02:00 2020-12-11 17:03:30 IP Address     70.203.63.0       100
#> # ℹ 73 more rows
#> # ℹ 12 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>, exclusions <chr>
```

## Excluding observations

The `exclude_*()` suite of function will return a data frame that has
removed observations that match the exclusion criteria. Exclude
functions print their own messages about the number of observations
excluded.

``` r

# Exclude survey responses used to preview the survey
df %>%
  exclude_preview() %>%
  dplyr::glimpse()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> Rows: 98
#> Columns: 16
#> $ StartDate               <dttm> 2020-12-11 12:17:22, 2020-12-11 12:17:41, 202…
#> $ EndDate                 <dttm> 2020-12-11 12:21:41, 2020-12-11 12:22:07, 202…
#> $ Status                  <chr> "IP Address", "IP Address", "IP Address", "IP …
#> $ IPAddress               <chr> "73.23.43.0", "16.140.105.0", "107.57.244.0", …
#> $ Progress                <dbl> 100, 100, 100, 100, 100, 100, 100, 100, 100, 1…
#> $ `Duration (in seconds)` <dbl> 651, 409, 140, 213, 177, 662, 296, 277, 54, 43…
#> $ Finished                <lgl> TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
#> $ RecordedDate            <dttm> 2020-12-11 12:21:42, 2020-12-11 12:22:07, 202…
#> $ ResponseId              <chr> "R_fbYBeNscosfzN9L", "R_yyG1HGXOMNPfWDn", "R_9…
#> $ LocationLatitude        <dbl> 34.03852, 44.96581, 27.98064, 29.76499, 40.335…
#> $ LocationLongitude       <dbl> -118.25739, -93.07187, -82.78531, -95.36156, -…
#> $ UserLanguage            <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN"…
#> $ Browser                 <chr> "Chrome", "Chrome", "Chrome", "Chrome", "Chrom…
#> $ Version                 <chr> "87.0.4280.88", "87.0.4280.88", "87.0.4280.88"…
#> $ `Operating System`      <chr> "Windows NT 10.0", "Windows NT 10.0", "Windows…
#> $ Resolution              <chr> "1366x768", "1536x864", "1536x864", "1366x768"…
```

Piping will apply subsequent excludes to the data frames with the
previous excludes already applied. Therefore, it often makes sense to
remove the preview surveys and incomplete surveys before checking other
exclusion types to speed processing.

``` r

# Exclude preview then incomplete progress rows then duplicate locations and IP addresses
df %>%
  exclude_preview() %>%
  exclude_progress() %>%
  exclude_duplicates(print = FALSE)
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows with incomplete progress were excluded, leaving 92 rows.
#> ℹ 9 out of 92 duplicate rows were excluded, leaving 83 rows.
```

## Messages and console output

Messages about the number of rows meeting the exclusion criteria are
printed to the console by default. These messages are generated by the
`mark_*()` functions and carry over to `check_*()` functions. They can
be turn off by setting `quiet` to `TRUE`.

``` r

# Turn off marking/checking messages with quiet = TRUE
df %>%
  check_progress(quiet = TRUE)
#> # A tibble: 6 × 16
#>   StartDate           EndDate             Status     IPAddress    Progress
#>   <dttm>              <dttm>              <chr>      <chr>           <dbl>
#> 1 2020-12-17 15:40:53 2020-12-17 15:43:25 IP Address 22.51.31.0         99
#> 2 2020-12-17 15:40:56 2020-12-17 15:46:23 IP Address 71.146.112.0        1
#> 3 2020-12-17 15:41:52 2020-12-17 15:46:37 IP Address 15.223.0.0         13
#> 4 2020-12-17 15:41:27 2020-12-17 15:46:45 IP Address 19.127.87.0        48
#> 5 2020-12-17 15:49:42 2020-12-17 15:51:50 IP Address 40.146.247.0        5
#> 6 2020-12-17 15:49:28 2020-12-17 15:55:06 IP Address 2.246.67.0         44
#> # ℹ 11 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>
```

Note that `exclude_*()` functions have the `mark_*()` messages turned
off by default and produce their own messages about exclusions. To
silence these messages, set `silent` to `TRUE`.

``` r

# Turn off exclusion messages with silent = TRUE
df %>%
  exclude_preview(silent = TRUE) %>%
  exclude_progress(silent = TRUE) %>%
  exclude_duplicates(silent = TRUE)
#> # A tibble: 83 × 16
#>    StartDate           EndDate             Status     IPAddress    Progress
#>    <dttm>              <dttm>              <chr>      <chr>           <dbl>
#>  1 2020-12-11 12:17:22 2020-12-11 12:21:41 IP Address 73.23.43.0        100
#>  2 2020-12-11 12:17:41 2020-12-11 12:22:07 IP Address 16.140.105.0      100
#>  3 2020-12-11 12:19:45 2020-12-11 12:23:16 IP Address 107.57.244.0      100
#>  4 2020-12-11 12:37:51 2020-12-11 12:43:09 IP Address 15.232.167.0      100
#>  5 2020-12-11 12:37:04 2020-12-11 12:45:50 IP Address 98.75.201.0       100
#>  6 2020-12-11 12:55:35 2020-12-11 12:59:24 IP Address 51.113.171.0      100
#>  7 2020-12-11 13:22:34 2020-12-11 13:35:19 IP Address 17.163.199.0      100
#>  8 2020-12-11 16:59:08 2020-12-11 17:02:05 IP Address 84.56.189.0       100
#>  9 2020-12-11 17:02:00 2020-12-11 17:03:30 IP Address 70.203.63.0       100
#> 10 2020-12-11 17:01:32 2020-12-11 17:09:41 IP Address 33.185.89.0       100
#> # ℹ 73 more rows
#> # ℹ 11 more variables: `Duration (in seconds)` <dbl>, Finished <lgl>,
#> #   RecordedDate <dttm>, ResponseId <chr>, LocationLatitude <dbl>,
#> #   LocationLongitude <dbl>, UserLanguage <chr>, Browser <chr>, Version <chr>,
#> #   `Operating System` <chr>, Resolution <chr>
```

Though `exclude_*()` functions do not print the data frame to the
console, `mark_*()` and `check_*()` do. To avoid printing to the
console, set `print` = `FALSE`.

``` r

# Turn off marking/checking printing data frame with print = FALSE
df %>%
  check_progress(print = FALSE)
#> ℹ 6 out of 100 rows did not complete the study.
```

## Deidentifying data

By default, Qualtrics records participant IP address and location.[^2]
You can also record properties of the participants’ computers such as
operating system, web browser type and version, and screen
resolution.[^3] While these pieces of information can be useful for
excluding observations, they carry potentially identifiable information.
Therefore, you may want to remove them from data frame before saving or
processing it. The
[`deidentify()`](https://docs.ropensci.org/excluder/reference/deidentify.md)
function removes potentially identifiable data columns collected by
Qualtrics. By default, the function uses a strict rule to remove IP
address, location, and computer information (browser type and version,
operating system, and screen resolution).

``` r

# Exclude preview then incomplete progress rows
df %>%
  exclude_preview() %>%
  exclude_progress() %>%
  exclude_duplicates() %>%
  deidentify() %>%
  dplyr::glimpse()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows with incomplete progress were excluded, leaving 92 rows.
#> ℹ 9 out of 92 duplicate rows were excluded, leaving 83 rows.
#> Rows: 83
#> Columns: 8
#> $ StartDate               <dttm> 2020-12-11 12:17:22, 2020-12-11 12:17:41, 202…
#> $ EndDate                 <dttm> 2020-12-11 12:21:41, 2020-12-11 12:22:07, 202…
#> $ Status                  <chr> "IP Address", "IP Address", "IP Address", "IP …
#> $ Progress                <dbl> 100, 100, 100, 100, 100, 100, 100, 100, 100, 1…
#> $ `Duration (in seconds)` <dbl> 651, 409, 140, 213, 662, 296, 277, 54, 432, 10…
#> $ Finished                <lgl> TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
#> $ RecordedDate            <dttm> 2020-12-11 12:21:42, 2020-12-11 12:22:07, 202…
#> $ ResponseId              <chr> "R_fbYBeNscosfzN9L", "R_yyG1HGXOMNPfWDn", "R_9…
```

If the computer information is not considered sensitive, it can be kept
by setting the `strict` argument to `FALSE`, thereby only removing IP
address and location.

``` r

# Exclude preview then incomplete progress rows
df %>%
  exclude_preview() %>%
  exclude_progress() %>%
  exclude_duplicates() %>%
  deidentify(strict = FALSE) %>%
  dplyr::glimpse()
#> ℹ 2 out of 100 preview rows were excluded, leaving 98 rows.
#> ℹ 6 out of 98 rows with incomplete progress were excluded, leaving 92 rows.
#> ℹ 9 out of 92 duplicate rows were excluded, leaving 83 rows.
#> Rows: 83
#> Columns: 12
#> $ StartDate               <dttm> 2020-12-11 12:17:22, 2020-12-11 12:17:41, 202…
#> $ EndDate                 <dttm> 2020-12-11 12:21:41, 2020-12-11 12:22:07, 202…
#> $ Status                  <chr> "IP Address", "IP Address", "IP Address", "IP …
#> $ Progress                <dbl> 100, 100, 100, 100, 100, 100, 100, 100, 100, 1…
#> $ `Duration (in seconds)` <dbl> 651, 409, 140, 213, 662, 296, 277, 54, 432, 10…
#> $ Finished                <lgl> TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
#> $ RecordedDate            <dttm> 2020-12-11 12:21:42, 2020-12-11 12:22:07, 202…
#> $ ResponseId              <chr> "R_fbYBeNscosfzN9L", "R_yyG1HGXOMNPfWDn", "R_9…
#> $ Browser                 <chr> "Chrome", "Chrome", "Chrome", "Chrome", "Edge"…
#> $ Version                 <chr> "87.0.4280.88", "87.0.4280.88", "87.0.4280.88"…
#> $ `Operating System`      <chr> "Windows NT 10.0", "Windows NT 10.0", "Windows…
#> $ Resolution              <chr> "1366x768", "1536x864", "1536x864", "1366x768"…
```

[^1]: Though the functions take data frames input, they produce
    [tibbles](https://tibble.tidyverse.org/).

[^2]: To avoid recording this potentially identifiable information, go
    to *Survey options* \> *Security* \> *Anonymize responses*.

[^3]: You can turn these on by adding a new question (usually to the
    beginning of your survey) and choosing *Meta info* as the question
    type.
