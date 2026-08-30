# Changelog

## excluder (development version)

## excluder 0.5.2

CRAN release: 2025-05-15

#### MINOR IMPROVEMENTS

- User is alerted when applying
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md)
  when columns are already renamed.

#### PACKAGE DEVELOPMENT

- Tests for `*_ip()` have been updated to account for new IP addresses
  outside of US.

- Examples requiring internet access are now interactive.

- Tests requiring internet access are now skipped on CI in addition to
  CRAN.

#### BUG FIXES

- Fixed bug where data frames missing resolution columns could not
  rename columns.

#### DOCUMENTATION UPDATES

- URLs for `{rgeolocate}` package now point toward URL for package
  archive.

## excluder 0.5.1 (2024-01-13)

CRAN release: 2024-01-13

#### NEW FEATURES

- Added new `qualtrics_fetch2` dataset that includes labels as column
  names.

#### PACKAGE DEVELOPMENT

- The {tidyselect} package has deprecated using the `.data` pronoun in
  tidy selections. So `.data` is no longer used in `select()` or other
  selection functions.
- We used the
  [`usethis::use_tidy_description()`](https://usethis.r-lib.org/reference/tidyverse.html)
  to tidy up DESCRIPTION file.

#### BUG FIXES

- Fixed bug to now reassign column names when `rename = TRUE`.

#### DOCUMENTATION UPDATES

- We have switched from the {devtools} to the {remotes} version of
  `install_github()` in the README to reduce dependencies.
- Because the {iptools} package has been removed from CRAN, the links to
  {iptools} functions now refer to the GitHub site.

## excluder 0.5.0 (2023-02-13)

CRAN release: 2023-02-13

#### PACKAGE DEVELOPMENT

- The `{iptools}` package has been replaced with the
  [`{ipaddress}`](https://github.com/davidchall/ipaddress) package.

#### DOCUMENTATION UPDATES

- The *Getting started* vignette has been renamed *excluder*.
- The data sets have been added to the References page.

## excluder 0.4.0 (2022-06-22)

CRAN release: 2022-06-22

#### NEW FEATURES

- The
  [`use_labels()`](https://docs.ropensci.org/excluder/reference/use_labels.md)
  function extracts the Qualtrics question labels included when
  importing with `qualtRics::fetch_survey()` and renames the column
  names with these labels.
- The `qualtrics_fetch` data set offers an example data set that is
  imported using the `qualtRics::fetch_survey()` function.
- The
  [`rename_columns()`](https://docs.ropensci.org/excluder/reference/rename_columns.md)
  function renames the columns to match standard Qualtrics column names.
  This is primarily a utility function for the `mark_*()` functions but
  may be useful to users in some circumstances.

#### MINOR IMPROVEMENTS

- For excluding based on screen resolution, previously you had to
  provide a minimum width or height. This works fine if all screens are
  in the same orientation, but if screens differ in their orientation,
  this isn’t sufficient. Now there is an overall minimum argument
  `res_min` that will apply to both width and height. That is, now you
  can test if at least one of the dimensions has a resolution greater
  than `res_min`. ([\#9](https://github.com/ropensci/excluder/issues/9))

#### DOCUMENTATION UPDATES

- To use several of the exclusion criteria, you must collect
  non-anonymized data and/or computer metadata. The documentation now
  points to the Qualtrics documentation about non-anonymized data and
  computer metadata.

#### PACKAGE DEVELOPMENT

- The package now has a logo!
- The number of CRAN downloads is now indicated by a badge.
- Tests for helper functions have been improved.

## excluder 0.3.3 (2021-12-03)

CRAN release: 2021-12-03

#### BUG FIXES

- Updating [dplyr](https://dplyr.tidyverse.org) to 1.0.8 caught problem
  with using `across()` inside
  [`is.na()`](https://rdrr.io/r/base/NA.html). Instead, it now uses
  `if_all()` with [`is.na()`](https://rdrr.io/r/base/NA.html) as
  argument. Thanks to
  [@romainfrancois](https://github.com/romainfrancois) for pull request
  [\#7](https://github.com/ropensci/excluder/pull/7).
- [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  now properly converts numeric data in Status and Finished columns.

## excluder 0.3.2 (2021-11-10)

CRAN release: 2021-11-18

#### MINOR IMPROVEMENTS

- The
  [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  function can now rename columns to match the default column names used
  in all of the verb function arguments.
- The `mark_ip_()` function now checks for (1) internet
  connectivity, (2) whether the IP address data can be downloaded from
  <https://www.iwik.org/ipcountry/>, and (3) if the country code is
  valid. The function fails gracefully if any of these are not met.

#### DOCUMENTATION UPDATES

- The README and vignette have clarified that multiple `check_*()`
  functions should not be piped.
- The `*_ip()` function documentation has clarified the internet
  connectivity requirements.

#### PACKAGE DEVELOPMENT

- The paper for the [excluder](https://docs.ropensci.org/excluder/)
  package has now been published in [Journal of Open Source
  Software](https://doi.org/10.21105/joss.03893). The JOSS badge has
  been added to the README.
- Tests for `*_ip()` functions are now skipped on CRAN to avoid timeout
  delays.
- Clean up package in preparation for submission to CRAN.

## excluder 0.3.1 (2021-11-04)

#### MINOR IMPROVEMENTS

- The messages now use [cli](https://cli.r-lib.org) to generate messages
  that more clearly outputs numbers and text.
- All functions now print the output data frames by default but can all
  be turned off with `print = FALSE`.
- `*_ip()` functions now include `include_na` as an argument (like
  `*_duplicates()` and `*_location()` functions), so users can decide
  whether to include NA values in the data that meet exclusion criteria.
- There are now five new utility functions that help simplify the
  primary verb functions. `keep_exclusion_column()` allows users to keep
  the exclusion column in the output from `check_*()` functions and
  moves the column the first column in the output.
  [`mark_rows()`](https://docs.ropensci.org/excluder/reference/mark_rows.md)
  does the bulk of the work creating new columns for exclusion criteria
  and marking rows that meet the criteria.
  [`print_data()`](https://docs.ropensci.org/excluder/reference/print_data.md)
  controls whether the output is printed to the console.
  [`print_exclusion()`](https://docs.ropensci.org/excluder/reference/print_exclusion.md)
  generates the message about how many rows were excluded by the
  `exclude_*()` functions.
  [`validate_columns()`](https://docs.ropensci.org/excluder/reference/validate_columns.md)
  validates the number, names, and type of columns that are inputted as
  arguments in the verb functions.
- The NEWS.md file is now based on the rOpenSci template.

#### BUG FIXES

- The
  [`unite_exclusions()`](https://docs.ropensci.org/excluder/reference/unite_exclusions.md)
  function now properly removes multiple separators when multiple
  exclusion criteria are used.
- The
  [`mark_duplicates()`](https://docs.ropensci.org/excluder/reference/mark_duplicates.md)
  function now properly counts and includes the correct number of NAs
  for both IP addresses and locations and properly prints data.

#### PACKAGE DEVELOPMENT

- The [excluder](https://docs.ropensci.org/excluder/) package has now
  been approved by and transferred to [rOpenSci](https://ropensci.org/).
  The package was peer reviewed by Joseph O’Brien
  ([@jmobrien](https://github.com/jmobrien)) and Julia Silge
  ([@juliasilge](https://github.com/juliasilge)), who are now listed as
  reviewers in the DESCRIPTION file.

## excluder 0.3.0 (2021-10-13)

#### NEW FEATURES

- The `mark_durations()` function now marks fast and slow durations
  separately.
- The primary functionality of the package has moved from the
  `check_*()` functions to the `mark_*()` functions. Thus, `check_*()`
  and `exclude_*()` now first call `mark_*()` to mark the rows with
  exclusion criteria, then filter the excluded rows. The documentation
  for `check_*()` and `exclude_*()` now inherit the arguments from
  `mark_*()`. This change has been updated in the README and Getting
  Started vignette.

#### MINOR IMPROVEMENTS

- `exclude__*()` functions now have `print = FALSE` and `quiet = TRUE`
  set as default argument values.
- Calls to [`rbind()`](https://rdrr.io/r/base/cbind.html) have been
  replaced with `bind_cols()` and
  [`dplyr::pull()`](https://dplyr.tidyverse.org/reference/pull.html) has
  been replaced with `[[]]`.
- Calls to `all_of()` and `any_of()` now refer to
  [tidyselect](https://tidyselect.r-lib.org) rather than
  [dplyr](https://dplyr.tidyverse.org).
- `if()` statements are now more robust by using
  [`identical()`](https://rdrr.io/r/base/identical.html) rather than
  `==` and `&&` instead of `&`.
- The [stringr](https://stringr.tidyverse.org) package is now imported
  instead of suggested.
- All mark, check, and exclude functions for a particular exclusion type
  have been combined into a single R file. So now each exclusion type
  has its own R file. Similarly, data file scripts have been combined
  into a single file.

#### BUG FIXES

- The `*_ip()` functions and documentation have been updated to fix a
  bug/typo to clarify that they mark, check, and exclude rows with IP
  addresses outside of the specified country.

#### DEPRECATED AND DEFUNCT

- [`collapse_exclusions()`](https://docs.ropensci.org/excluder/reference/collapse_exclusions.md)
  has been renamed
  [`unite_exclusions()`](https://docs.ropensci.org/excluder/reference/unite_exclusions.md)
  to match [tidyverse](https://tidyverse.tidyverse.org) terminology.
  [`collapse_exclusions()`](https://docs.ropensci.org/excluder/reference/collapse_exclusions.md)
  is now deprecated and will be removed in a future version, use
  `unite_exlusions()`. `unite_exlusions()` also switched from using NA
  to “” for rows with no exclusions. Combined columns now no longer have
  leftover separators.

#### DOCUMENTATION FIXES

- Package links are replaced with external URLs.

## excluder 0.2.2

- Lifecycle has been updated to Stable, and the repo status of Active
  has been added.
- There is now a Getting Started Excluding Data vignette that gives an
  overview of package usage.
- Users can now specify the separator used between exclusion types in
  the
  [`collapse_exclusions()`](https://docs.ropensci.org/excluder/reference/collapse_exclusions.md)
  function. They can also opt to not remove the excluded columns.
- A bug in the
  [`collapse_exclusions()`](https://docs.ropensci.org/excluder/reference/collapse_exclusions.md)
  function was fixed, so now a subset of exclusion columns can be
  collapsed into a single column.
- A bug in the
  [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  function was fixed, so now the Finished column is converted to a
  logical vector.
- A codemeta.json file was created.
- URLs have been replaced or tweaked to address CRAN package check
  warnings.
- Functions are now organized by topic in the Reference page of the
  website.

## excluder 0.2.1

- The argument name for the data frame switched from `.data` to `x` to
  avoid confusion with the [rlang](https://rlang.r-lib.org) use of
  `.data`.
- Instead of using `quo()` and `sym()` to create new names for columns
  used as arguments, `.data[[var]]` is now used.
- The `dupe_count` column was removed from
  [`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md)
  output. Tests were adjusted to account for the new number of columns.
- [`check_duplicates()`](https://docs.ropensci.org/excluder/reference/check_duplicates.md)
  now specifies the number of NA columns.
- URLs have been replaced or tweaked to address CRAN package check
  warnings.
- Links function reference pages have been added to the README.

## excluder 0.2.0

- The
  [`deidentify()`](https://docs.ropensci.org/excluder/reference/deidentify.md)
  function was added, which removes IP address, location, and computer
  information columns.

## excluder 0.1.0

- The `check_qualtrics` argument was removed from
  [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  because the functionality did not make sense. This breaks backwards
  compatibility.
- [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  now only filters out label rows if label rows are present and outputs
  invisibly.
- Tests were added for the `qualtrics_raw` dataset and the
  [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  function.
- Package-level documentation was created.

## excluder 0.0.1

- [`remove_label_rows()`](https://docs.ropensci.org/excluder/reference/remove_label_rows.md)
  now converts character columns to dates for multiple date formats,
  including YYYY-MM-DD HH:MM:SS, YYYY-MM-DD HH:MM, MM:DD:YYYY HH:MM:SS,
  and MM:DD:YYYY HH:MM
  ([\#1](https://github.com/ropensci/excluder/issues/1)).
- Code of Conduction and Contributor Guide are added.
- Citation and Contributor sections are added to README.

## excluder 0.0.0.1

- Initial GitHub release
