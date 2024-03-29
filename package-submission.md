# (PART) Package Submissions

# Overview

The following page gives an overview of the submission process along
with key principles to follow. See also \[Package
Guidelines\]\[guidelines\] for package specific guidelines and
requirement and the \[*Bioconductor* new package submission
tracker\]\[tracker\].

# Bioconductor Package Submissions

-   [Introduction](#subintro)
-   [Types of Packages](#type)
-   [Package Naming Policy](#naming)
-   [Author/Maintainer Expectations](#author)
-   [Submission](#submission)
-   [Experiment data package](#experPackage)
-   [Annotation package](#annPackage)
-   [Workflow package](#workPackage)
-   [Review Process](#whattoexpect)
-   [Following Acceptance](#afteraccept)
-   [Additional Support](#support)

## Introduction

To submit a package to *Bioconductor* the package should:

-   Address areas of high-throughput genomic analysis, e.g., sequencing,
    expression and other microarrays, flow cytometry, mass spectrometry,
    image analysis; see \[biocViews\]\[\].
-   Interoperate with other *Bioconductor* packages by *re-using common
    data structures* (see \[Common Bioconductor Methods and
    Classes\]\[bioc-common\]) and existing infrastructure (e.g.,
    `rtracklayer::import()` for input of common genomic files).
-   Adopt software best practices that enable reproducible research and
    use, such as full documentation and vignettes (including fully
    evaluated code) as well as commitment to long-term user support
    through the *Bioconductor* \[support site\]\[\].
-   Not exist on CRAN. A package can only be submitted to one or the
    other.
-   Comply with \[Package Guidelines\]\[guidelines\].
-   Your package cannot depend on any package (or version of a package)
    that is not (yet) available on CRAN or *Bioconductor*. The package
    should work with whatever current version of the package is
    publically available.

## Types of Packages

*Bioconductor* packages are broadly defined by four main package types:
\[**Software**\]\[software-pkgs\], \[**Experiment
Data**\]\[exptdata-pkgs\], \[**Annotation**\]\[annotation-pkgs\] and
\[**Workflow**\]\[workflow-pkgs\].

-   Software Packages. Most packages contributed by users are software
    packages. Software packages provide implementation of algorithms
    (e.g. statistical analysis), access to resources (e.g. biomart, or
    NCBI) or visualizations (e.g. volcano plots, pathways plots).
    Instructions for creating Software packages can be found here:
    \[Package guidelines\]\[guidelines\].

-   [Annotation packages](#annPackage) are database-like packages that
    provide information linking identifiers (e.g., Entrez gene names or
    Affymetrix probe ids) to other information (e.g., chromosomal
    location, Gene Ontology category). It is also encouraged to utilize
    AnnotationHub for storage and access to large raw data files and
    their conversion to standard R formats. Instructions for adding data
    to AnnotationHub and designing a annotation package to use
    AnnotationHub can be found here: \[Create A Hub Package\]\[\].

-   [Experiment data packages](#experPackage) provide data sets that are
    used, often by software packages, to illustrate particular analyses.
    These packages contain curated data from an experiment, teaching
    course or publication and in most cases contain a single data set.
    It is also encouraged to utilize ExperimentHub for storage and
    access to larger data files. ExperimentHub is also particularly
    useful for hosting collections of related data sets. Instructions
    for adding data to ExperimentHub and designing an experiment data
    package to use ExperimentHub can be found here: \[Create A Hub
    Package\]\[\].

-   [Workflow packages](#workPackage) contain vignettes that describe a
    bioinformatics workflow that involves multiple Bioconductor
    packages. These vignettes are usually more extensive than the
    vignettes that accompany software packages. These packages do not
    need man/ or R/ directories nor a data/ directory as ideally
    workflows make use of existing data in a Bioconductor package. See
    development section on \[Workflows\]\[\] for more details.

See \[Package Guidelines\]\[guidelines\] for details on package format
and syntax.

## Package Naming Policy

Package naming:

1.  Ownership of package name. Bioconductor follows \[CRAN’s
    policy\]\[\] in requiring that contributors give the right to use
    the package name to Bioconductor at time of submission, so that the
    Bioconductor team can orphan the package and allow another
    maintainer to take it over in the event that the package contributor
    discontinues package maintenance. See Bioconductor’s package
    \[end-of-life policy\]\[\] for more details.

2.  Uniqueness of package name. Packages should be named in a way that
    does not conflict (irrespective of case) with any current or past
    BIOCONDUCTOR package, nor any current or past CRAN package.

See [Package naming guidelines](#package-name) for more guidelines.

## Author / Maintainer Expectations

Acceptance of packages into *Bioconductor* brings with it ongoing
package maintenance responsibilities. Package authors are expected to:

-   Follow Bioconductor guidelines

    These include standard \[guidelines\]\[guidelines\], \[version
    numbering\]\[versioning\], [coding style](#r-code), [code
    performance requirements](#correctness-space-and-time), [memory
    usage](#memory), \[using existing data classes\]\[bioc-common\], and
    the other requirements described below.

-   Follow the release cycle of Bioconductor

    There are two releases each year, around April and October. The
    \[release schedule\]\[release-schedule\] will indicate the
    timetables and deadlines for each release. A release cycle typically
    produces two versions of packages, ‘devel’ and ‘release’. It is
    important to be familiar with these branch concepts. Once your
    package has been accepted, it will initially be in the ‘devel’
    branch. The current devel branch becomes the next release. Most
    users are expected to use the release branch, so they will not
    immediately have access to your package until the next release. Bug
    fixes can be fixed in both branches, while new features should only
    be added to the ‘devel’ branch.

-   Maintain the package using version control

    Realize that *Bioconductor*, unlike CRAN, maintains all package
    source code under git version control. This means that you make
    changes to your package using \[git\]\[gitver\]. If your package is
    accepted, you will receive instructions with typical git operations
    (see the after [acceptance section](#afteraccept)). Package
    maintenance through software release cycles, including prompt
    updates to software and documentation, is needed due to possible
    underlying changes in R and/or other package dependencies.

-   Check Build Reports and Fix Issues Promptly

    *Bioconductor* has weekly to daily \[build reports\]\[\] for all
    package types. It is the maintainers responsibility to check the
    build reports and respond to any ERROR/Warning/Notes produced by the
    install, build, or check process of a package. The maintainer will
    receive automatic build notifications when the package starts to
    fail on linux from `BBS-noreply@bioconductor.org`; maintainers
    should check that emails from this email address can be delivered.

-   Subscribe to the \[bioc-devel\]\[bioc-devel-mail\] mailing list

    The Bioconductor team communicates with developers through this
    list. It is also a good channel to communicate changes to other
    developers. Addressing Bioconductor team requests in a timely manner
    guarantees that your package remains available through Bioconductor.

-   \[Register\]\[support-register\] on and use the \[support site\]\[\]

    The support site is the official support channel for users. Users
    and even developers may ask questions regarding your package on this
    platform. Be sure to include all the packages that you maintain in
    the “Watched Tags” section of your support site profile. This will
    notify you of any questions posted regarding your package(s). It is
    important to promptly respond to bug reports and questions either on
    the \[*Bioconductor* support site\]\[support site\] post or directly
    to developers. Some maintainers prefer to indicate a `BugReports:`
    field in their package’s DESCRIPTION file. This field indicates a
    particular web page for submitting bug reports and questions.

## Submission

-   Read and follow the full \[Contributor Guidelines\]\[guidelines\]
    section.

-   Submit by opening a new issue in the *Bioconductor*
    \[Contributions\]\[\] repository, following the
    \[guidelines\]\[tracker\] of the `README.md` file. Assuming that
    your package is in a \[GitHub Repository\]\[git-repo-create\] and
    under the default branch, add the link to your repository to the
    issue you are opening. You cannot specify any alternative branches;
    the default branch is utilized. The default branch must contain only
    package code. Any files or directories for other applications
    (Github Actions, devtool, etc) should be in a different branch. If
    you are submitting two highly related packages or circular dependent
    packages please see \[here\]\[cirdep\]. The lighter dependent or
    package that can be installed without a dependency should be
    submitted first; this is generally the associated data package to a
    software package.

## Experiment Data Packages

Experimental data packages contain data specific to a particular
analysis or experiment. They often accompany a software package for use
in the examples and vignettes and in general are not updated regularly.
If you need a general subset of data for workflows or examples first
check the AnnotationHub resource for available files (e.g., BAM, FASTA,
BigWig, etc.) or ExperimentHub for available processed example data set
already included in *Bioconductor*. If no current files or data sets are
appropriate consider an associated Experiment Data Package that utilizes
*[ExperimentHub](https://bioconductor.org/packages/3.15/ExperimentHub)*.

If you have an associated data package for your software package, please
do *NOT* create a separate issue in the our tracker repository for that.
Instead, please add the data package repository to the same issue as the
software package. The process for doing this is documented
\[here\]\[cirdep\]. Generally the data package should be submitted
first.

The *[HubPub](https://bioconductor.org/packages/3.15/HubPub)* package is
helpful for creating a template for a hub package. The vignette \[Create
A Hub Package\]\[\] provides full details.

## Annotation Packages

Annotation packages contain lightly or non-curated data from a public
source and are updated with each *Bioconductor* release (every 6
months). They are a source of general annotation for one or many
organisms and are not specific to a particular experiment. When
possible, they should support the `select()` interface from
AnnotationDbi.

Annotation packages should *NOT* be posted to the tracker repository.
Instead send an email to <packages@bioconductor.org> with a description
of the proposed annotation package and futher instructions of where to
send the package will be provided. Whenever possible Annotation Packages
should use the
*[AnnotationHub](https://bioconductor.org/packages/3.15/AnnotationHub)*
for managing files.

The *[HubPub](https://bioconductor.org/packages/3.15/HubPub)* package is
helpful for creating a template for a hub package. The vignette \[Create
A Hub Package\]\[\] provides full details.

<a name="workPackage"></a>

## Worflow Packages

Workflow packages contain vignettes that describe a bioinformatics
workflow that involves multiple Bioconductor packages. These vignettes
are usually more extensive than the vignettes that accompany software
packages. These packages do not need man/ or R/ directories nor a data/
directory as ideally workflows make use of existing data in a
Bioconductor package. See development section on \[Workflows\]\[\] for
more details

<a name="whattoexpect"></a>

## Review Process

-   A new package is initially labeled as `1. awaiting moderation`. A
    *Bioconductor* team member will take a very brief look at your
    package, to ensure that it is intended for *Bioconductor*.
    Appropriate packages will be re-labelled `2. review in progress` and
    a reviewer will be automatically assigned. Your assigned reviewer
    will address your concerns and help you through the review process.
    The entire review process typically takes between 2 and 6 weeks. If
    there is no response after 3 to 4 weeks, package reviewers may close
    the issue until further updates, changes, and/or commentary are
    received.

-   If in the pre-review process there are identified larger issues with
    the package, a label `3e. pending pre-review changes` or a more
    specific flag of package issue will be assigned. Please address any
    pre-review identified issues and comment back for the package
    admininstrators to re-evaluate.

-   Please be courteous with your package reviewers and always follow
    the *Bioconductor* \[Code Of Conduct\]\[\] with correspondence.
    Please allow 2-3 weeks for reviewers to assess your package.

-   The package will be submitted to the *Bioconductor* build system
    (BBS). The system will check out your package from GitHub and move
    it to our git.bioconductor.org git server. Please familiarize
    yourself with \[git\]\[gitver\] as the git.bioconductor.org is the
    versions the (BBS) will always use. It will then run `R CMD build`
    to create a ‘tarball’ of your source code, vignettes, and man pages.
    It will run `R CMD check` on the tarball, to ensure that the package
    conforms to standard *R* programming best practices. *Bioconductor*
    has chosen to utilize a custom `R CMD check` environment; See \[R
    CMD check environment\]\[\] for more details. Finally, the build
    system will run `BiocCheckGitClone()` and
    `BiocCheck('new-package'=TRUE)` to ensure that the package conforms
    to *Bioconductor*
    *[BiocCheck](https://bioconductor.org/packages/3.15/BiocCheck)*
    standards. The system will perform these steps using the \[‘devel’
    version\]\[\] of *Bioconductor*, on three platforms (Linux, Mac OS
    X, and Windows). After these steps are complete, a link to a build
    report will be appended to the new package issue. Avoid surprises by
    running these checks on your own computer, under the ‘devel’ version
    of *Bioconductor*, before submitting your package.

-   If the build report indicates problems, modify your package and
    commit changes to the git.bioconductor.org version of your package
    as described in the [new package git
    workflow](#new-package-workflow). If there are problems that you do
    not understand, seek help on the \[bioc-devel\]\[bioc-devel-mail\]
    mailing list.

-   To trigger a new build, include a version bump in your commit, e.g.,
    from `Version: 0.99.0` to `Version: 0.99.1`. Pre-release versions
    utilize the `0.99.z` format. When accepted and released, your
    package’s version number will be automatically incremented to 1.0.0.

-   Once your package builds and checks without errors or (avoidable)
    warnings, a *Bioconductor* team member will provide a technical
    review of your package. Other *Bioconductor* developers and users
    with domain expertise are encouraged to provide additional community
    commentary. Reviewers will add comments to the issue you created.

-   Respond to the issues raised by the reviewers. You *must* respond to
    the primary reviewer, and are strongly encouraged to consider
    community commentary. Typically your response will involve code
    modifications; commit these to the default branch of
    git.bioconductororg. When you have addressed all concerns, add a
    comment to the issue created in step 2 to explain your response.

-   The reviewer will assess your responses, perhaps suggesting further
    modifications or clarification. The reviewer will then accept your
    package for inclusion in *Bioconductor*, or decline it. The label
    `2. review in progress` will be replaced by `3a. accepted` or
    `3b. declined`.

-   If your package is accepted, it will be added to *Bioconductor*’s
    nightly ‘devel’ builds. All packages in the ‘devel’ branch of the
    repository are ‘released’ to the user community once every six
    months, in approximately April and October.

-   Once the review process is complete, the issue you created will be
    closed. All updates to your package will be through the
    \[*Bioconductor* Git Server\]\[gitver\].

## Following Acceptance

Following acceptance of a package:

-   Packages accepted on the tracker repository are added to the ‘devel’
    branch of the *Bioconductor* GIT repository, with the current
    version number of the accepted package.
-   Packages are then built by the *Bioconductor* nightly build process.
    On-demand builds of accepted packages do not occur. Please see the
    \[build reports\]\[\] for how often the different package types are
    built. The changes pushed to a ‘devel’ version of a package can take
    up to 24-28 hours to appear. See \[build timings\]\[\]. If the build
    is successful, the package has its own ‘landing page’ created, and
    the package is made available to users of the ‘devel’ branch of
    *Bioconductor* via `BiocManager::install()`.
-   Changes to their package (if any), should be done to version in the
    \[*Bioconductor* git server\]\[gitver\].
-   Developers should bump the `z` portion of their version number every
    time they commit changes to their package, following the \[Version
    numbering\]\[versioning\] guidelines. If developers don’t bump the
    version, the changes made to their package *do not propagate* to the
    *Bioconductor* web site and package repository.

## Additional Support

We are eager to enhance the quality and interoperability of
*Bioconductor* software and will provide additional support when
requested by package developers. Example areas of assistance include use
of appropriate S4 structures, specific guidance on efficient
implementation, guidance on code structure, and critical assessment of
package documentation and structure. Use the
\[bioc-devel\]\[bioc-devel-mail\] mailing list or email
<maintainer@bioconductor.org> to obtain additional support.

-   Support Email: <maintainer@bioconductor.org>
