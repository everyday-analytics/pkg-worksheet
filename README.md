
Introduction Step 00: Press play on the video guide Step 0: Have some
functions that you’d like to package up like the below from Hester’s
‘note’ example Step 1: Create package architecture using
usethis::create\_package() Step 2: Describe what the package does in the
DESCRIPTION file Step 3: Add your functions to an R Script saved in the
R folder Step 4: Make the package “active” and test your functions
interactively with devtools::load\_all() Step 5: Run a check on the new
package using devtools::check() Step 6: Add needed dependencies Step 6a.
Declare dependencies using usethis::use\_package(“package\_name”) Step
6b: Add dependencies and export functions in R script using “\#’
@importFrom and \#’ @export” Step 7: Document these additions using a
‘Roxygen’ skeleton Step 8: Incorporate documentation additions into
package using devtools::document() Repeat Step 5 devtools::check() Step
9: Create a readme file with usethis::use\_readme\_md() Step 10: Create
some relevent tests with usethis::use\_test() Beyond 20 minutes Step 11:
Create a license using usethis::use\_\*\_license() Step 12: Build and
install package using devtools::build() Step 13: Sharing online? 13.a
Initialize your directory as git repository 13.b Push your package to
your github account. Step 14: Tell the world how to get your package
with devtools::install\_github() More topics you might consider
investigating: Additional Resources Additional tools used to create this
guide

<!-- README.md is generated from README.Rmd. Please edit that file -->

# note

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
<!-- badges: end -->

The goal of note is to play notes from R.

``` r
library(note)
note("A")
```

# How we write the package…

``` r
usethis::create_package(".")
```

``` r
rate <- 44100
multiplier <- 2 * pi / rate
bpm <- 80
default_volume <- 5

notes <- c("A" = 0, "A#" = 1, "Bb" = 1, "B" = 2, "Cb" = 2, "B#" = 3, "C" = 3,
  "C#" = 4, "Db" = 4, "D" = 5, "D#" = 6, "Eb" = 6, "E" = 7, "Fb" = 7, "E#" = 8,
  "F" = 8, "F#" = 9, "Gb" = 9, "G" = 10, "G#" = 11, "Ab" = 11)

calc_frequency <- function(note, octave) {
  # 440hz is A above middle C
  440 * 2^((unname(notes[note]) + (octave * 12)) / 12)
}

calc_volume <- function(x) {
  # x should be between 1 and 10
  stopifnot(x >= 1, x <= 10)
  
  x / 10
}

calc_length <- function(rate, length, bpm) {
  seq(1, as.integer(rate * length * 60 / bpm))
}

calc_multiplier <- function(rate) {
  2 * pi / rate
}

#' Title
#'
#' @param note 
#' @param length 
#' @param octave 
#' @param volume 
#'
#' @return
#' @export
#'
#' @examples
#' note("A")
note <- function(note, length = 1, octave = 0, volume = default_volume) {
  frequency <- calc_frequency(note, octave)
  volume <- calc_volume(volume)
  length <- calc_length(rate, length, bpm)
  multiplier <- calc_multiplier(rate)
  res <- sin(frequency * multiplier * length) * volume
  structure(res, class = "note")
}

print.note <- function(x, ...) {
  audio::play(x, ...)
}
```

# time to test

``` r
note("A")
```

``` r
readme2pkg::chunk_to_r("hesterfunctions")
usethis::use_package("audio")
usethis::use_mit_license()
```

``` r
devtools::check()
devtools::build()
```
