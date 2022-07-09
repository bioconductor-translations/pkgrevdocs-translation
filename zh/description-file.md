# DESCRIPTION文件

`DESCRIPTION`文件必须以正确的格式编写。 在下面的章节中，我们将介绍`DESCRIPTION`文件中的字段以及相关文件。

## `Package`

此字段应包含软件包的名字。 注意软件仓库（如GitHub）和软件包的名字应该一致（其中大小写也应该一致）。

## `Title`

这是一小段关于软件包简单描述的短句。

## `Version`

所有\[*Bioconductor*\]软件包使用`x.y.z`版本号命名规范。 请参见 \[版本控制\]以了解Bioconductor是如何对release分支和devel分支进行版本控制的。 如果软件包是第一次提交到Bioconductor，那么其版本号应该被设为`0.99.0`。

如下规则适用于版本号的控制：

-   `x`一般被设为0，如果软件包还没有被正式发布。
-   在release分支中，`y`应该被设置为偶数，而在devel分支中，y应该被设置为奇数。 一般来说，开发者无需自行增加y的值，在每次Bioconductor新版本发布时，y的值会自动加1。
-   每当有新的改动提交到Bioconductor上时，`z`需要加1。

## `Description`

此字段包含了对软件包详细的描述。文字描述无需太长，但是其必须涵盖关于软件包功能的详细描述。 一般至少需要包含三个完整的句子。

## `Authors@R`

应该使用`Authors@R`而不是Authors字段。 此处应该填写软件包的维护者，其角色(例如`cre`，作者)和其常用邮箱。 对于将来在软件包中出现的任何问题，用户可以会通过此邮箱和维护者联系。

如果软件包作者有[ORCiD](https://orcid.org/)标识符，那么标识符可以在person()函数中可以通过一个名为ORCID的元素并通过comment参数设置。

    person("Lori", "Shepherd",
      email = Lori.Shepherd@roswellpark.org,
      role = c("cre", "aut"),
      comment = c(ORCID = "0000-0002-5910-4010"))

Only one person should be listed as the `Maintainer` to ensure a single point of contact. This person by default will have commit access to the git repository on `git.bioconductor.org`. Commit access can be given to other developers by request on the \[bioc-devel\]\[bioc-devel-mail\] mailing list.

Another option is to add \[collaborators to the
<i class="fab fa-github"></i> GitHub\]\[Github collaborators\] repository. This approach enables development by many but restricts push access to `git.bioconductor.org`.

## `License`

The license field should preferably refer to a standard license (see \[wikipedia\]\[wiki-license\]) using one of
<i class="fab fa-r-project"></i>’s standard specifications.
<i class="fab fa-r-project"></i> ships with the following \[standard licenses\]\[R License\]

Be specific about any version that applies (e.g., `GPL-2`). Licenses restricting use, e.g., to academic or non-profit researchers, are not suitable for \[*Bioconductor*\]\[\]. Core Bioconductor packages are typically licensed under `Artistic-2.0`.

To specify a non-standard license, include a file named `LICENSE` in your package (containing the full terms of your license) and use the string `file LICENSE` in this `License:` field.

The package should contain only code that can be redistributed according to the package license. Be aware of the licensing agreements for packages you are depending on in your package. Not all packages are open source even if they are publicly available.

## `LazyData`

如果软件包额外包含了数据文件，我们建议不要使用`LazyData: TRUE`。 根据我们的经验，对于很大的数据文件，设置LazyData为TRUE只会使软件包的载入变得很慢。 当然如果有例外的情形的话，请在提交软件包时同时提交此例外的详细描述。

## `Depends`, `Imports`, `Suggests`, `Enhances`

所有依赖的软件包必须存在于Bioconductor的biocViews列表中，或者存在于CRAN上。Bioconductor不支持Remotes字段，因此Bioconductor不允许依赖于只存在于其他软件仓库（例如GitHub）的软件包。

直接使用其他软件包中经过广泛测试的功能，而不是重新编写具有相同的功能的代码。 使用Bioconductor中广泛使用的软件包（如*[biomaRt](https://bioconductor.org/packages/3.15/biomaRt)*， *[AnnotationDbi](https://bioconductor.org/packages/3.15/AnnotationDbi)*，或者 *[Biostrings](https://bioconductor.org/packages/3.15/Biostrings)*） 和定义了基础类的软件包（如*[SummarizedExperiment](https://bioconductor.org/packages/3.15/SummarizedExperiment)*， *[GenomicRanges](https://bioconductor.org/packages/3.15/GenomicRanges)*::GRanges， *[S4Vectors](https://bioconductor.org/packages/3.15/S4Vectors)*::Rle或者 *[Biostrings](https://bioconductor.org/packages/3.15/Biostrings)*::DNAStringSet）。请避免重复编写其他软件包已经实现的功能。 请见\[基础Bioconductor方法和类\]\[bioc-common\]. Bioconductor的审核人在这点上会非常严格！ 新的软件包应该很好和现有的Bioconductor中已定义的类交互和兼容。不应该重复实行已有的功能，尤其是在数据导入方面。

依赖的软件包只能在`Depends:`，`Imports:`, `Suggests:`或者`Enhances:`中出现一次。 下面的规则可以来指导如何确定依赖包的类型：

-   `Imports:` 字段中列出的依赖包应该只在软件包命名空间内部使用，例如依赖包中定义的方法和类。 一般来说，大部分依赖包都在此列出。

-   `Depends:`字段中列出的依赖包应该提供软件包使用的关键功能并且这些关键功能直接被用户使用，例如`GenomicRanges`包被列在`GenomicAlignments`包的Depends中。 一般来说，Depends字段包含超过三个依赖包不常见。

-   `Suggests:` is for packages used in vignettes, examples, and in conditional code.Commonly, annotation and experiment packages (e.g., `TxDb*`) used in vignette and example code are included in this field thus avoiding a costly download. In the case where an external one-off function is required for package code, the package availability and usage can be done via:
  
        if (!requireNamespace('suggPKG', quietly = TRUE))
            stop("Install 'suggPKG' to use this function.")
        suggPKG::function()

-   `Enhances:` is for packages such as `Rmpi` or `parallel` that enhance the performance of your package, but are not strictly needed for its functionality.

It is seldom necessary to specify <i class="fab fa-r-project"></i> or specific versions as dependencies, since the Bioconductor release strategy and standard installation instructions guarantee these constraints. Repositories mirrored outside Bioconductor should include branches for each Bioconductor release, and may find it useful to fully specify versions to enforce constraints otherwise guaranteed by Bioconductor installation practices.

For additional information regarding Depends, Imports, Suggest and where a package should be placed see [Connecting to other packages](https://kbroman.org/pkg_primer/pages/depends.html) and the [package dependency](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-Dependencies) section of \[Writing R Extensions\]\[\].

## `SystemRequirements`

This field is for listing any external software which is required, but not automatically installed by the normal package installation process.

If the installation process is non-trivial, a top-level [`INSTALL` file](#sysdep) should be included to document the process. If a user facing [`README`](#readme) is included it is also recommended to document the process there; do not try to install a dependency for a user anywhere in the package (i.e. readme, r code, man pages, vignette). You may show instructions only in unevaluated sections.

## `biocViews`

这个字段是必须的！

Specify at least two leaf node from \[biocViews\]\[\]. Multiple leaf terms are encouraged but terms must come from the same trunk or package type (i.e., `Software`, `AnnotationData`, `ExperimentData`, or `Workflow`). `biocViews` terms are case-sensitive.

The field name “biocViews” is case-sensitive and must begin with a lower-case ‘b’. Please use a single line with biocViews seperated by commas

(i.e,`biocViews: GeneTarget, SingleCell`).

## `BugReports`

鼓励提供GitHub的相关链接，用来让用户报告关于软件包的任何问题。

## `URL`

This field directs users to source code repositories, additional help resources, etc; details are provided in the \[Writing R Extensions\]\[\] manual, `RShowDoc("R-exts")`.

## `Video`

此字段包含教学视频的链接。

## `Collates`

This may be necessary to order class and method definitions appropriately during package installation.

## `BiocType`

如果提交`Docker`或者`Workflow`类型的软件包，此字段是必须的。 否则，此字段可以可选的定义软件包的类型，例如`Software`，`ExperimentData`或者`Annotation`。
