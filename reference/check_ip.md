# Check for IP addresses from outside of a specified country.

The `check_ip()` function subsets rows of data, retaining rows that have
IP addresses from outside the specified country. The function is written
to work with data from [Qualtrics](https://www.qualtrics.com/) surveys.

## Usage

``` r
check_ip(
  x,
  id_col = "ResponseId",
  ip_col = "IPAddress",
  rename = TRUE,
  country = "US",
  include_na = FALSE,
  keep = FALSE,
  quiet = FALSE,
  print = TRUE
)
```

## Arguments

- x:

  Data frame or tibble (preferably imported from Qualtrics using
  {qualtRics}).

- id_col:

  Column name for unique row ID (e.g., participant).

- ip_col:

  Column name for IP addresses.

- rename:

  Logical indicating whether to rename columns (using
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md))

- country:

  Two-letter abbreviation of country to check (default is "US").

- include_na:

  Logical indicating whether to include rows with NA in IP address
  column in the output list of potentially excluded data.

- keep:

  Logical indicating whether to keep or remove exclusion column.

- quiet:

  Logical indicating whether to print message to console.

- print:

  Logical indicating whether to print returned tibble to console.

## Value

An object of the same type as `x` that includes the rows with IP
addresses outside of the specified country. For a function that marks
these rows, use
[`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md).
For a function that excludes these rows, use
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md).

## Details

To record this information in your Qualtrics survey, you must ensure
that [Anonymize responses is
disabled](https://www.qualtrics.com/support/survey-platform/survey-module/survey-options/survey-protection/#AnonymizingResponses).

Default column names are set based on output from the
[`qualtRics::fetch_survey()`](https://docs.ropensci.org/qualtRics/reference/fetch_survey.html).
The function uses
[`ipaddress::country_networks()`](https://davidchall.github.io/ipaddress/reference/country_networks.html)
to assign IP addresses to specific countries using [ISO 3166-1 alpha-2
country codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

The function outputs to console a message about the number of rows with
IP addresses outside of the specified country. If there are `NA`s for IP
addresses (likely due to including preview data—see
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md)),
it will print a message alerting to the number of rows with `NA`s.

## Note

This function **requires internet connectivity** as it uses the
[`ipaddress::country_networks()`](https://davidchall.github.io/ipaddress/reference/country_networks.html)
function, which pulls daily updated data from
<https://www.iwik.org/ipcountry/>. It only updates the data once per
session, as it caches the results for future work during the session.

## See also

Other ip functions:
[`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md),
[`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md)

Other check functions:
[`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md),
[`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md),
[`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md),
[`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md),
[`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md),
[`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)

## Examples

``` r
if (FALSE) { # interactive()
# Check for IP addresses outside of the US
data(qualtrics_text)
check_ip(qualtrics_text)

# Remove preview data first
qualtrics_text %>%
  exclude_preview() %>%
  check_ip()

# Check for IP addresses outside of Germany
qualtrics_text %>%
  exclude_preview() %>%
  check_ip(country = "DE")

# Do not print rows to console
qualtrics_text %>%
  exclude_preview() %>%
  check_ip(print = FALSE)

# Do not print message to console
qualtrics_text %>%
  exclude_preview() %>%
  check_ip(quiet = TRUE)
}
```
