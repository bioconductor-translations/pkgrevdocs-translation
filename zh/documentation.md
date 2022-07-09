# 帮助文档

软件包文档对于用户理解如何使用你的代码 非常重要。

## Bioconductor文档基本要求：

-   a \[vignette\]\[CRAN vigs\] in Rmd or Rnw format with executable code that demonstrates how to use the package to accomplish a task,

-   \[man pages\]\[CRAN Rd\] for all exported functions with runnable examples, well documented data structures especially if not a \[pre-exiting class\]\[bioc-common\]

-   well documented datasets for data provided in `data/` and in `inst/extdata/`.

References to the methods used as well as to other similar or related projects and packages is also expected.

If data structures differ from similar packages, \[*Bioconductor* reviewers\]\[reviewer-list\] will expect some justification as to why. Keep in mind it is always possible to extend existing classes.

## Vignettes

A vignette demonstrates how to accomplish non-trivial tasks embodying the core functionality of your package. There are two common types of vignettes.

-   A *Sweave* vignette is an `.Rnw` file that contains $\\LaTeX$ and chunks of <i
    class="fab fa-r-project"></i> code. The code chunk starts with a line `<<>>=`, and ends with `@`. Each chunk is evaluated during `R CMD build`, prior to $\\LaTeX$ compilation to a PDF document.

-   An *R markdown* vignette is similar to a *Sweave* vignette, but uses [markdown](http://daringfireball.net/projects/markdown/) instead of $\\LaTeX$ for structuring text sections and resulting in HTML output. The `r   BiocStyle::CRANpkg("knitr")` package can process most *Sweave* and all *R markdown* vignettes, producing pleasing output. Refer to \[Writing package vignettes\]\[CRAN vigs\] for technical details. See the `r   BiocStyle::Biocpkg("BiocStyle")` package for a convenient way to use common macros and a standard *Bioconductor* style vignette.

A vignette provides reproducibility: the vignette produces the same results as copying the corresponding commands into an
<i class="fab fa-r-project"></i> session. It is therefore **essential** that the vignette embed executed <i
class="fab fa-r-project"></i> code. Shortcuts (e.g., using a $\\LaTeX$ verbatim environment, or using the *Sweave* `eval=FALSE` flag, or equivalent tricks in markdown) undermine the benefit of vignettes and are generally **not allowed**; exceptions can be made with proper justification and are at the discretion of \[*Bioconductor* reviewers\]\[reviewer-list\].

All packages are required to have at least one Rmd or Rnw vignette. Vignettes go in the `vignettes/` directory of the package. Vignettes are often used as standalone documents, so best practices are to include an informative title, the primary author of the vignette, the last modification date of the vignette, and a link to the package landing page. We encourage the use of `r BiocStyle::Biocpkg("BiocStyle")` for formatting with `html_document` as rendering target. Something like the following in the vignette will accomplish the above suggestion:

    output:
      BiocStyle::html_document:
        toc: true
        toc_depth: 2

Some best practices and requirements for writing *Bioconductor* vignettes are detailed in the following sections.

### Introduction

Add an “Introduction” section that serves as an abstract to introduce the objective, models, unique functions, key points, etc that distinguish the package from other packages in the same area. This is a requirement of Bioconductor package vignettes. It should include a short motivation for the package in general as well as motivation for inclusion of the package in Biconductor. When relevant, a brief review and comparison of packages with similar functionality or scope should be provided either in the Introduction or in a separate dedicated vignette section.

### Installation

Add an “Installation” section that show to users how to download and load the package from Bioconductor.

These instructions and any installations instructions should be in an `eval=FALSE` code chunk. No where in the code (<i class="fab fa-r-project"></i> code, man pages, vignettes, Rmd files) should someone try to install or download system dependencies, applications, packages, etc. Developers can provide instructions to follow in unevaluated code chunks, and should assume all necessary dependencies, applications or packages are already set up on a user’s system.

### Table of contents

If appropriate, we strongly encourage a table of contents

### Evaluated code chunks

Non-trival executable code is a must!!!

Static vignettes are not acceptable.

### Session information

Include a section with the `SessionInfo()` at the end of the vignette.

### `vignettes/` directory and intermediate files

Only the source vignette file (`.Rnw` or `.Rmd`) and any necessary static images should be in the vignette directory. No intermediate files should be present. This include complete processed vignette products as well; the vignette should be created through the `R CMD build` of a package. To include other types of documentation please use the `inst/doc` or other appropriately named `inst` directory.

### References

Remember to include any relevant references to methods.

## Man pages

See the \[Writing R Extensions section on man pages\]\[CRAN Rd\] for detailed instruction or format information for documenting a package, functions, classes, and data sets.

All help pages should be comprehensive.

### Package-level documentation

\[*Bioconductor*\]\[\] encourages having a package man page with an overview of the package and links to the main functions. Users should be able to have a relevant page display with `?<package name>`

### Functions and classes

All exported functions and classes need will have a man page. Man pages describing new classes must be very detailed on the structure and the type of information that is stored.

### Data

Data man pages must include provenance information and data structure information.

### Examples

All man pages should have an runnable examples.

The use of `donttest` and `dontrun` is discouraged and generally not allowed; exceptions can be made with proper justification and are at the discretion of \[*Bioconductor* reviewers\]\[reviewer-list\].

If this option is used it will also be preferable to use `donttest` instead of `dontrun`; `donttest` requires valid
<i class="fab fa-r-project"></i> code while `dontrun` does not.

## The `inst/script/` directory

The scripts in this directory can vary.

Most importantly if data was included in the `inst/extdata/` directory, a related script must be present in this directory documenting very clearly how the data was generated and source information.

It should include source URLs and any key information regarding filtering or processing.

It can be executable code, sudo code, or a text description.

Users should be able to download and be able to roughly reproduce the file or object that is present as data.

## Other

Other types of documentation (e.g. static files, jupyter notebooks, etc.) can be provided through `inst` subdirectories but do not substitute for the [*Bioconductor* documentation requirements](#doc-require) listed above.
