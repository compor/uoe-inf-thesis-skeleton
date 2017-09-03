# cmake file

cmake_minimum_required(VERSION 2.8)

project(thesis NONE)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/cmake-pandocology/")

include(pandocology)

#

set(SRC_DIR "src")
set(STYLE_DIR "style")
set(FIGURES_DIR "figures")
set(BIBLIOGRAPHY_DIR "bib")

set(TEMPLATE "${STYLE_DIR}/template.tex")
set(PREAMBLE "${STYLE_DIR}/preamble.tex")
set(CITATION_STYLE "${STYLE_DIR}/ref_format.csl")
set(METADATA "${SRC_DIR}/metadata.yaml")

set(BIBLIOGRAPHY "${BIBLIOGRAPHY_DIR}/references.bib")

file(GLOB MARKDOWN_SOURCES
  RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/${SRC_DIR}/*.md)

add_document(
  TARGET thesis
  OUTPUT_FILE thesis.pdf
  SOURCES ${METADATA} ${MARKDOWN_SOURCES}
  RESOURCE_FILES ${METADATA}
  RESOURCE_DIRS ${STYLE_DIR} ${FIGURES_DIR} ${BIBLIOGRAPHY_DIR}
  PANDOC_DIRECTIVES
  -H ${PREAMBLE}
  --template=${TEMPLATE}
  --bibliography=${BIBLIOGRAPHY}
  --csl=${CITATION_STYLE}
  --highlight-style pygments
  -N
  --latex-engine=xelatex)

