# Package index

## Check functions

- [`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md)
  : Check for duplicate IP addresses and/or locations
- [`check_duration()`](https://docs.ropensci.org/excluder/reference/check_duration.md)
  : Check for minimum or maximum durations
- [`check_ip()`](https://docs.ropensci.org/excluder/reference/check_ip.md)
  : Check for IP addresses from outside of a specified country.
- [`check_location()`](https://docs.ropensci.org/excluder/reference/check_location.md)
  : Check for locations outside of the US
- [`check_preview()`](https://docs.ropensci.org/excluder/reference/check_preview.md)
  : Check for survey previews
- [`check_progress()`](https://docs.ropensci.org/excluder/reference/check_progress.md)
  : Check for survey progress
- [`check_resolution()`](https://docs.ropensci.org/excluder/reference/check_resolution.md)
  : Check screen resolution

## Exclude functions

- [`exclude_duplicates()`](https://docs.ropensci.org/excluder/reference/exclude_duplicates.md)
  : Exclude rows with duplicate IP addresses and/or locations
- [`exclude_duration()`](https://docs.ropensci.org/excluder/reference/exclude_duration.md)
  : Exclude rows with minimum or maximum durations
- [`exclude_ip()`](https://docs.ropensci.org/excluder/reference/exclude_ip.md)
  : Exclude IP addresses from outside of a specified country.
- [`exclude_location()`](https://docs.ropensci.org/excluder/reference/exclude_location.md)
  : Exclude locations outside of US
- [`exclude_preview()`](https://docs.ropensci.org/excluder/reference/exclude_preview.md)
  : Exclude survey previews
- [`exclude_progress()`](https://docs.ropensci.org/excluder/reference/exclude_progress.md)
  : Exclude survey progress
- [`exclude_resolution()`](https://docs.ropensci.org/excluder/reference/exclude_resolution.md)
  : Exclude unacceptable screen resolution

## Mark functions

- [`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md)
  : Mark duplicate IP addresses and/or locations
- [`mark_duration()`](https://docs.ropensci.org/excluder/reference/mark_duration.md)
  : Mark minimum or maximum durations
- [`mark_ip()`](https://docs.ropensci.org/excluder/reference/mark_ip.md)
  : Mark IP addresses from outside of a specified country.
- [`mark_location()`](https://docs.ropensci.org/excluder/reference/mark_location.md)
  : Mark locations outside of US
- [`mark_preview()`](https://docs.ropensci.org/excluder/reference/mark_preview.md)
  : Mark survey previews
- [`mark_progress()`](https://docs.ropensci.org/excluder/reference/mark_progress.md)
  : Mark survey progress
- [`mark_resolution()`](https://docs.ropensci.org/excluder/reference/mark_resolution.md)
  : Mark unacceptable screen resolution

## Helpers

- [`deidentify()`](https://docs.ropensci.org/excluder/reference/deidentify.md)
  : Remove columns that could include identifiable information
- [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  : Remove two initial rows created in Qualtrics data
- [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md)
  : Rename columns to match standard Qualtrics names
- [`unite_exclusions()`](https://docs.ropensci.org/excluder/reference/unite_exclusions.md)
  : Unite multiple exclusion columns into single column
- [`use_labels()`](https://docs.ropensci.org/excluder/reference/use_labels.md)
  : Use Qualtrics labels as column names

## Data sets

- [`qualtrics_fetch`](https://docs.ropensci.org/excluder/reference/qualtrics_fetch.md)
  :

  Example numeric metadata imported with `qualtRics::fetch_survey()`
  from simulated Qualtrics study

- [`qualtrics_fetch2`](https://docs.ropensci.org/excluder/reference/qualtrics_fetch2.md)
  :

  Example numeric metadata imported with `qualtRics::fetch_survey()`
  from simulated Qualtrics study but with labels included as column
  names

- [`qualtrics_numeric`](https://docs.ropensci.org/excluder/reference/qualtrics_numeric.md)
  : Example numeric metadata from simulated Qualtrics study

- [`qualtrics_raw`](https://docs.ropensci.org/excluder/reference/qualtrics_raw.md)
  : Example text-based metadata from simulated Qualtrics study

- [`qualtrics_text`](https://docs.ropensci.org/excluder/reference/qualtrics_text.md)
  : Example text-based metadata from simulated Qualtrics study
