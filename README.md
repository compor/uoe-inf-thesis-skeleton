
# University of Edinburgh Informatics thesis template

## Introduction

This is a skeleton directory for typesetting an example of a thesis following the School of Informatics, 
University of Edinburgh, stylistic criteria.

## Build status

[![Build Status](https://travis-ci.org/compor/uoe-inf-thesis-skeleton.svg?branch=master)](https://travis-ci.org/compor/uoe-inf-thesis-skeleton)

## Requirements

- [pandoc](https://github.com/jgm/pandoc)
- LaTeX distribution like `TeX Live` (used by `pandoc`)
- `cmake` 2.8 and above
- for further dependencies check the submodules used:
  - [UoE Informatics thesis LaTeX class](https://github.com/dhil/inf-thesis-latex-cls)
  - [Tom's PhD thesis template](https://github.com/tompollard/phd_thesis_markdown)
  - [pandocology](https://github.com/jeetsukumaran/cmake-pandocology)

## Building

To build perform the following steps:

- `git clone --recursive https://github.com/compor/uoe-inf-thesis-skeleton.git`
- `mkdir build && cd build`
- `cmake [path to this source repo]`
- `source exports.sh && make`
- open `product/thesis.pdf` with a PDF viewer

The `exports.sh` is required to set the `TEXMFLOCAL` environmental variable for the specific `LaTeX` classes and style 
files used.


## Credits

- Daniel for providing the UoE Informatics latex and class files [online](https://github.com/dhil/inf-thesis-latex-cls)
- Tom for providing his more comprehensive [template](https://github.com/tompollard/phd_thesis_markdown). Please follow
  the instructions in this page to provide appropriate credit.
- [pandocology](https://github.com/jeetsukumaran/cmake-pandocology)

