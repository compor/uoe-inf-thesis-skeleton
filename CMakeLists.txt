# cmake file

cmake_minimum_required(VERSION 2.8)

project(thesis NONE)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/cmake-pandocology/")
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/inf-thesis-latex-cls/")

include(pandocology)
include(uoe-infthesis-latex-cls)

#

# set target names as var due to reuse
set(SHIELD_TARGET "thesis-eushield")
set(LATEX_CLASS_TARGET "thesis-class")


add_uoe_eushield(TARGET ${SHIELD_TARGET})

add_custom_command(TARGET ${SHIELD_TARGET}
  POST_BUILD 
  COMMAND texhash ${CMAKE_CURRENT_BINARY_DIR}/${SHIELD_TARGET})


add_uoe_infthesis(TARGET ${LATEX_CLASS_TARGET})

add_custom_command(TARGET ${LATEX_CLASS_TARGET}
  POST_BUILD 
  COMMAND texhash ${CMAKE_CURRENT_BINARY_DIR}/${LATEX_CLASS_TARGET})


set(TEXMFLOCAL_PATH1 "${CMAKE_CURRENT_BINARY_DIR}/${SHIELD_TARGET}")
set(TEXMFLOCAL_PATH2 "${CMAKE_CURRENT_BINARY_DIR}/${LATEX_CLASS_TARGET}")

file(TO_NATIVE_PATH ${TEXMFLOCAL_PATH1} TEXMFLOCAL_NPATH1)
file(TO_NATIVE_PATH ${TEXMFLOCAL_PATH2} TEXMFLOCAL_NPATH2)

set(TEXMFLOCAL_PATH "${TEXMFLOCAL_NPATH1}:${TEXMFLOCAL_NPATH2}:")

set(EXPORTS_SCRIPT "exports.sh")

configure_file(
  "${CMAKE_SOURCE_DIR}/utils/scripts/${EXPORTS_SCRIPT}.in" 
  ${EXPORTS_SCRIPT} @ONLY)

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
  --filter pandoc-citeproc
  --highlight-style pygments
  -N
  --latex-engine=xelatex
  DEPENDS thesis-eushield thesis-class)

