# Using the ‘Devel’ Version of *Bioconductor*

## Which version of R?

Package authors should develop against the version of *R* that will be available to users when the *Bioconductor* devel branch becomes the *Bioconductor* release branch.

*R* has a ‘.y’ release in x.y.z every year (typically mid-April), but *Bioconductor* has a .y release (where current devel becomes release) every 6 months (mid-April and mid-October).

This means that, from mid-October through mid-April, *Bioconductor* developers should be developing against R-devel. From mid-April to mid-October, developers should use R-release (actually, the R snapshot from the R-x-y-branch) for *Bioconductor* development.

See the \[BiocManager\]\[\] vignette for instructions on using multiple versions of *Bioconductor*.

## Using ‘bioc-devel’ during mid-April to mid-October

In order to use the ‘bioc-devel’ version of *Bioconductor* during the mid-April to mid-October release cycle, use the release version of *R* and invoke the function `install(version="devel")` (from the \[BiocManager\]\[\] package):

    if (!requireNamespace("BiocManager", quietly=TRUE))
        install.packages("BiocManager")
    BiocManager::install(version = "devel")
    BiocManager::valid()              # checks for out of date packages

After doing this, all packages will be installed from the ‘bioc-devel’ repository.

## Using ‘bioc-devel’ during mid-October to mid-April

In order to use the ‘bioc-devel’ version of *Bioconductor* during the mid-October to mid-April release cycle, you must install the devel version of *R*.

-   [Source](https://stat.ethz.ch/R/daily/)
-   [macOS](https://mac.r-project.org/)
-   [Windows](https://cran.r-project.org/bin/windows/base/rdevel.html)

Then, make sure that your version of \[BiocManager\]\[\] is current and your packages up-to-date.

    if (!requireNamespace("BiocManager", quietly=TRUE))
        install.packages("BiocManager")
    BiocManager::install(version="devel")
    BiocManager::valid()
