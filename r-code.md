# R code

Everyone has their own coding style and formats. There are however some
best practice guidelines that \[*Bioconductor*
reviewers\]\[reviewer-list\] will look for.

<i class="fab fa-r-project"></i> can be a robust, fast and efficient
programming language but often some coding practices result in less than
ideal use of resources.

This section will review some key points, suggestions, and best
practices that will aid in the package review process and assist in
making code more robust and efficient.

## License

Only contain code that can be distributed under the license specified
(see also [The DESCRIPTION file](#description-license)).

## R Code Development

### Re-use of functionality, classes, and generics

Avoid re-implementing functionality or classes (see also [The
DESCRIPTION file](description.html#depends-imports-suggests-enhances)).
Make use of appropriate existing packages (e.g.,
*[biomaRt](https://bioconductor.org/packages/3.15/biomaRt)*,
*[AnnotationDbi](https://bioconductor.org/packages/3.15/AnnotationDbi)*,
*[Biostrings](https://bioconductor.org/packages/3.15/Biostrings)*,
*[GenomicRanges](https://bioconductor.org/packages/3.15/GenomicRanges)*)
and classes (e.g.,
*[SummarizedExperiment](https://bioconductor.org/packages/3.15/SummarizedExperiment)*,
*[Biobase](https://bioconductor.org/packages/3.15/Biobase)*::AnnotatedDataFrame,
*[GenomicRanges](https://bioconductor.org/packages/3.15/GenomicRanges)*:;GRanges,
*[Biostrings](https://bioconductor.org/packages/3.15/Biostrings)*::DNAStringSet)
to avoid duplication of functionality available in other
\[*Bioconductor*\]\[\] packages. See also \[Common Bioconductor Methods
and Classes\]\[bioc-common\].

This encourages interoperability and simplifies your own package
development. If a new representation is needed, see [class development
section](#class-development). In general, \[*Bioconductor*\]\[\] will
insist on interoperability with [Common Classes](#commonclass) for
acceptance.

Developers should make an effort to re-use generics that fit the generic
contract for the proposed class-method pair i.e., the behavior of the
method aligns with the originally proposed behavior of the generic.
Specifically, the behavior can be one where the return value is of the
same class across methods. The method behavior can also be a performant
conceptual transformation or procedure across classes as described by
the generic.
*[BiocGenerics](https://bioconductor.org/packages/3.15/BiocGenerics)*
lists commonly used generics in Bioconductor. One example of a generic
and method implementation is that of the `rowSums` generic and the
corresponding method within the
*[DelayedArray](https://bioconductor.org/packages/3.15/DelayedArray)*
package. This generic contract returns a numeric vector of the same
length as the rows and is adhered to across classes including the
`DelayedMatrix` class. Re-using generics reduces the amount of new
generics by consolidating existing operations and avoids the mistake of
introducing a “new” generic with the same name. Generic name collisions
may mask or be masked by previous definitions in ways that are hard to
diagnose.

If there are problems, e.g., in performance or parsing your particular
file type, ask for input from other developers on the bioc-devel mailing
list. Common disadvantages to ‘implementing your own’ are the
introduction of non-standard data representations (e.g., neglecting to
translate coordinate systems of file formats to Bioconductor objects)
and user bewilderment.

### Naming of packages, functions, and classes

See section on [package naming](#package-name). The concepts there
should also apply to function names, class names, etc.

Avoid names that:

-   Are easily confused with existing package names, function names,
    class names.
-   Imply a temporal (e.g. `ExistingPackage2`) or qualitative
    (e.g. `ExistingPackagePlus`) relationship.
-   Suggest hate speech, slurs or profanity, either implicitly or
    explicitly.
-   Invoke or refer to any historical, ethical, or political contexts.
-   Reference well known people, characters, brands, places or icons.

### Methods development

We encourage maintainers to only create new methods for classes exported
within their packages. We discourage the generation of methods for
external classes, i.e., classes outside of the package
[`NAMESPACE`](#namespace). This can potentially cause method name
collisions (i.e., where two methods defined on the same object but in
different packages) and pollute the methods environment for those
external classes. New methods for established classes can also cause
confusion among users given that the new method and class definition are
in separate packages.

### File names

-   Filename extension for R code should be ‘.R’. Use the prefix
    ‘methods-’ for S4 class methods, e.g., ‘methods-coverage.R’. Generic
    definitions can be listed in a single file, ‘AllGenerics.R’, and
    class definitions in ‘AllClasses.R’.
-   Filename extension for man pages should be ‘.Rd’.

### Class development

Remember to **re-use** \[common *Bioconductor* methods and
classes\]\[bioc-common\] before implementing new representations. This
encourages interoperability and simplifies your own package development.

#### Class Names

Use CamelCaps: initial upper case, then alternate case between words.

#### Class design

*Bioconductor* prefers the use of S4 classes over S3 class. Please use
the S4 representation for designing new classes. Again, we can not
emphasize enough to reuse existing class structure if at all possible.
If an existing class structure is not sufficient, it is also encouraged
to look at extending or containing an [existing *Bioconductor*
class](#commonclass).

If your data requires a new representation or function, carefully design
an S4 class or generic so that other package developers with similar
needs will be able to re-use your hard work, and so that users of
related packages will be able to seamlessly use your data structures. Do
not hesitate to ask on the Bioc-devel mailing list for advice.

For any class you define, implement and use a ‘constructor’ for object
creation. A constructor is usually plain-old-function (rather than,
e.g., a generic with methods). It provides documented and user-friendly
arguments, while allowing for developer-friendly implementation. Use the
constructor throughout your own code, examples, and vignette.

Implement a `show()` method to effectively convey information to your
users without overwhelming them with detail.

Accessors (simple functions that return components of your object)
rather than direct slot access (using `@`) help to isolate the
implementation of your class from its interface. Generally `@` should
only be used in an accessor, all other code should use the accessor. The
accessor does not need to be exported from the class if the user has no
need or business accessing parts of your data object.
Plain-old-functions (rather than generic + method) are often sufficient
for accessors; it’s often useful to employ (consistently) a lightweight
name mangling scheme (e.g., starting the accessor method name with a 2
or 3 letter acronym for your package) to avoid name collisions between
similarly named functions in other packages.

The following layout is sometimes used to organize classes and methods;
other approaches are possible and acceptable.

-   All class definitions in R/AllClasses.R
-   All generic function definitions in R/AllGenerics.R
-   Methods are defined in a file named by the generic function. For
    example, all `show` methods would go in R/show-methods.R.

A Collates: field in the DESCRIPTION file may be necessary to order
class and method definitions appropriately during package installation.

### Function development

#### Functions Names

Function names should follow the following conventions:

-   Use camelCase: initial lower case, then alternate case between
    words.
-   Do not use ‘.’ (in the S3 class system, `some(x)` where `x` is class
    `A` will dispatch to `some.A`).
-   Prefix non-exported functions with a ‘.’.

#### Functional Programming and Length

Guiding principle: Smaller functions are easier to read, test, debug and
reuse.

-   Avoid large chunks of repeated code. If code is being repeated this
    is generally a good indication a helper function could be
    implemented.

-   Excessively long functions should also be avoided. Write small
    functions.

-   It is best if each function has only one job that it needs to do.
    And it is also best if that function does that job in as few lines
    of code as possible. If you find yourself writing great long
    functions that extend for more than a screen, then you should
    probably take a moment to split it up into smaller helper functions.

-   Nesting functions should be avoiding. It unnecessarily increases the
    main function length and makes the main function harder to read.
    Helper functions and internal functions should avoid being nested
    and include at the end of the file or as a seperate file
    (e.g. internal.R, helpers.R, utils.R)

#### Function arguments

-   Argument names to functions should be descriptive and well
    documented.
-   Arguments should generally have default values.
-   Check arguments against a validity check or `stopifnot`

#### Variable names

-   Use camelCase: initial lowercase, then alternate case between words.

#### Additional files and dependencies

Do NOT install anything on a users system.

System dependencies, applications, and additionally needed packages
should be assumed already present on the user’s system.

If necessary, package maintainers should provide instructions for
download and setup, but should not execute those instructions on behalf
of a user. Complicated or additional system dependency instructions
could be part of the [README file](#readme) and/or [INSTALL
file](#sysdep). All package dependencies must actively be on CRAN or
\[*Bioconductor*\]\[\].

All system and package dependencies should be the latest publically
available version.

#### Namespaces

-   Import all symbols used from packages other than “base”. Except for
    default packages (base, graphics, stats, etc.) or when overly
    tedious, fully enumerate imports.
-   Export all symbols useful to end users. Fully enumerate exports.

### Coding Style

While it may seem arbitrary, being consistent with coding style helps
other understand, evaluate, and debug code easier. While these are
optional, it is highly encouraged.

#### Indentation

-   Use 4 spaces for indenting. No tabs.
-   No lines longer than 80 characters.

#### Use of space

-   Always use space after a comma. This: `a, b, c`.
-   No space around “=” when using named arguments to functions. This:
    `somefunc(a=1, b=2)`
-   Space around all binary operators: `a == b`.

#### Comments

-   Use “\##” to start full-line comments.
-   Indent at the same level as surrounding code.
-   Comments should be for clarification, notes, and documentation only.
-   Do not leave in commented out code chunks that are not utilized
-   Commenting TODO’s should be avoided in published package code

#### Additonal style and formatting references

-   \[formatR\]\[\] a package that assists with formatting.

-   [Hadley Wickhams’s R Style Guide](https://style.tidyverse.org/).

## R Code Best Practices and Guidelines

Many common coding and sytax issues are flagged in `R CMD check` and
`BiocCheck()` (see the `R CMD check`
[cheatsheet](http://r-pkgs.had.co.nz/check.html) and
*[BiocCheck](https://bioconductor.org/packages/3.15/BiocCheck/vignettes/BiocCheck.html)*
vignette. Every effort should be made to clear up ERROR, WARNING, or
Notes produced from these checks.

### Code syntax and efficiency

-   Use `vapply()` instead of `sapply()`, and use the various `apply`
    functions instead of `for` loops. See below section on vectorize.
-   Use `seq_len()` or `seq_along()` instead of `1:...`. See below
    section on avoid `1:n` style iterations
-   Use `TRUE` and `FALSE` instead of `T` and `F`.
-   Use of numeric indices (rather than robust named indices).
-   Use `is()` instead of `class() ==` and `class() !=`.
-   Use `system2()` instead of `system()`.
-   Do not use `set.seed()` in any internal
    <i class="fab fa-r-project"></i> code.
-   Do not use `browser()` in any internal
    <i class="fab fa-r-project"></i> code.
-   Avoid the use of `<<-`.
-   Avoid use of direct slot access with `@` or `slot()`. Accessor
    methods should be created and utilized
-   Use the packages
    *[ExperimentHub](https://bioconductor.org/packages/3.15/ExperimentHub)*
    and
    *[AnnotationHub](https://bioconductor.org/packages/3.15/AnnotationHub)*
    instead of downloading external data from unsanctioned providers
    such as <i class="fab fa-github"></i> GitHub, &lt;i \* class=“fa
    fa-dropbox” aria-hidden=“true”&gt;</i> Dropbox, etc. In general,
    data utlilzed in packages should be downloaded from trusted public
    databases. See also section on web querying and file caching.
-   Use `<-` instead of `=` for assigning variables except in function
    arguments.

#### End-User messages

Use the functions `message()`, `warning()` and `error()`, instead of the
`cat()` function (except for customized `show()` methods). `paste0()`
should generally not be used in these methods except for collapsing
multiple values from a variable.

-   `message()` communicates diagnostic messages (e.g., progress during
    lengthy computations) during code evaluation.
-   `warning()` communicates unusual situations handled by your code.
-   `stop()` indicates an error condition.
-   `cat()` or `print()` are used only when displaying an object to the
    user in a `show` method.

#### Graphic Device

Use `dev.new()` to start a graphics drive if necessary. Avoid using
`x11()` or `X11()`, for it can only be called on machines that have
access to an X server.

#### Web Querying and File caching

Files downloaded should be cached. Please use
*[BiocFileCache](https://bioconductor.org/packages/3.15/BiocFileCache)*.
If a maintainer creates their own caching directory, it should utilize
standard caching directories
`tools::R_user_dir(package, which="cache")`. It is not allowed to
download or write any files to a users home directory, working
directory, or installed package directory. Files should be cached as
stated above with `BiocFileCache` (preferred) or `R_user_dir` or
`tempdir()/tempfile()` if files should not be persistent.

Please also follow guiding principles in Appendix section [Querying Web
Resources](#querying-web-resources), if applicable.

## Robust and Efficient R Code

*R* can be a robust, fast and efficient programming language, but some
coding practices can be very unfortunate. Here are some suggestions.

### Guiding Principles

1.  The primary principle is to make sure your code is **correct**. Use
    `identical()` or `all.equal()` to ensure correctness, and \[unit
    tests\]\[\] to ensure consistent results across code revisions.

2.  Write robust code. Avoid efficiencies that do not easily handle edge
    cases such as 0 length or `NA` values.

3.  Know when to stop trying to be efficient. If the code takes a
    fraction of a second to evaluate, there is no sense in trying for
    further improvement. Use `system.time()` or a package like
    \[microbenchmark\]\[\] to quantify performance gains.

### Reuse Existing Functionality and Classes

Emphasized again for good measure! Reuse existing import/export methods,
functionality, and classes whenever possible.

### Vectorize

Many R operations are performed on the whole object, not just the
elements of the object (e.g., `sum(x)` instead of
`x[1] + x[2] + x[2] + ...`). In particular, relatively few situations
require an explicit `for` loop.

Vectorize, rather than iterate (`for` loops, `lapply()`, `apply()` are
common iteration idioms in *R*). A single call `y <- sqrt(x)` with a
vector `x` of length `n` is an example of a vectorized function. A call
such as `y <- sapply(x, sqrt)` or a `for` loop
`for (i in seq_along(x)) y[i] <- sqrt(x[i])` is an iterative version of
the same call, and should be avoided. Often, iterative calculations that
can be vectorized are “hidden” in the body of a `for` loop

    for (i in seq_len(n)) {
        ## ...
        tmp <- foo(x[i])
        y <- tmp + ## ...
    }

and can be ‘hoisted’ out of the loop

    tmp <- foo(x)
    for (i in seq_len(n)) {
        ## ...
        y <- tmp[i] + ##
    }

Often this principle can be applied repeatedly, and an iterative `for`
loop becomes a few lines of vectorized function calls.

### ‘Pre-allocate and fill’ if iterations are necessary

Preallocate-and-fill (usually via `lapply()` or `vapply()`) rather than
copy-and-append. If creating a vector or list of results, use `lapply()`
(to create a list) or `vapply()` (to create a vector) rather than a
`for` loop. For instance,

    n <- 10000
    x <- vapply(seq_len(n), function(i) {
        ## ...
    }, integer(1))

manages the memory allocation of `x` and compactly represents the
transformation being performed. A `for` loop might be appropriate if the
iteration has side effects (e.g., displaying a plot) or where
calculation of one value depends on a previous value. When creating a
vector in a `for` loop, always pre-allocate the result

    x <- integer(n)
    if (n > 0) x[1] <- 0
    for (i in seq_len(n - 1)) {
        ## x[i + 1] <- ...
    }

**Never** adopt a strategy of ‘copy-and-append’

    not_this <- function(n) {
        x <- integer()
        for (i in seq_len(n))
            x[i] = i
        x
    }

This pattern copies the current value of `x` each time through the loop,
making `n^2 / 2` total copies, and scale very poorly even for trivial
computations:

    > system.time(not_this(1000)
       user  system elapsed
      0.004   0.000   0.004
    > system.time(not_this(10000))
       user  system elapsed
      0.169   0.000   0.168
    > system.time(not_this(100000))
       user  system elapsed
     22.827   1.120  23.936

### Avoid `1:n` style iterations

Write `seq_len(n)` or `seq_along(x)` rather than `1:n` or `1:length(x)`.
This protects against the case when `n` or `length(x)` is 0 (which often
occurs as an unexpected ‘edge case’ in real code) or negative.

### Parallel Recommendations

We recommend using
*[BiocParallel](https://bioconductor.org/packages/3.15/BiocParallel)*.
It provides a consistent interface to the user and supports the major
parallel computing styles: forks and processes on a single computer, ad
hoc clusters, batch schedulers and cloud computing. By default,
`BiocParallel` chooses a parallel back-end appropriate for the OS and is
supported across Unix, Mac and Windows. Coding requirements for
`BiocParallel` are:

-   Use `lapply()`-style iteration instead of explicit for loops.
-   The `FUN` argument to `bplapply()` must be a self-contained
    function; all symbols used in the function are from default R
    packages, from packages `require()`’ed in the function, or passed in
    as arguments.
-   Allow the user to specify the BiocParallel back-end. Do this by
    invoking `bplapply()` *without* specifying `BPPARAM`; the user can
    then override the default choice with `BiocParallel::register()`.

For more information see the \[BiocParallel vignette\]\[\].

When implementing in package development, a minimal number of cores (1
or 2) should be set as a default.
