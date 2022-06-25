# Long Tests

## What are they

Code in the `tests` subdirectory of all Bioconductor software packages is run by `R CMD check` on a daily basis as part of the \[Bioconductor nightly builds\]\[devel-software-build-report\]. The maximum amount of time that `R CMD check` is allowed to spend on each package is 40 min.

Package developers who wish to implement `tests` that will be too long to run in the context of the nightly builds can set up “long tests” in their package and add the package to the Bioconductor *Long Tests builds*.

## Setup

4 steps:

-   Put the code for the “long tests” in the `longtests` subdirectory of your package. Like for the code in the `tests` directory, this code will typically (but not necessarily) run \[unit tests\]\[\].

-   Add your package to the *Long Tests builds* by adding a `.BBSoptions` file to its top-level directory with the following line in it:
  
        RunLongTests: TRUE

-   The code in the `longtests` subdirectory will be run **once a week** (every Saturday) as part of the *Long Tests builds* and will be allowed to run for a **maximum of 6 hours** before a TIMEOUT is raised.

-   Check the \[latest *Long Tests* report\]\[devel-LongTests-report\] for BioC devel. The next time the report is updated (normally every Saturday), your package should be included in it.

Note that we also run the *Long Tests builds* for the current release branch once a week (every Saturday). The latest report for these builds is \[here\]\[release-LongTests-report\].

## “Short tests” vs “long tests”

The *Long Tests* setup forces developers to split the testing code in their package between “short tests” and “long tests”. The former go in the `tests` subdirectory and must be able to run in less than 40 min (this limit is actually for the full `R CMD check` command which runs other possibly time-consuming things in addition to the code in `tests`). The latter go in the `longtests` subdirectory and must be able to run in less than 6 hours.

Note that, unlike with “short tests” failures, “long tests” failures don’t prevent a package from propagating after a version bump. In other words, even if a package is included in the *Long Tests builds*, propagation is still determined by the results of the nightly builds.

## Need help?

Ask on the \[bioc-devel\]\[bioc-devel-mail\] mailing list if you have questions or need help with the *Long Tests builds*.
