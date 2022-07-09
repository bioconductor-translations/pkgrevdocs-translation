# 帮助文档

软件包文档对于用户理解如何使用你的代码非常重要。

## Bioconductor文档基本要求：

-   一个 \[vignette\]\[CRAN vigs\] 的Rmd 或 Rnw 格式文档，带有可执行的 代码，显示如何使用包来完成任务。

-   \[man pages\]\[CRAN Rd\] 记录所有导出的函数，并有它们的操作 示例，记录完善的数据结构，尤其是如果不是已经存在 \[pre-exiting class\]\[bioc-common\]的。

-   详细记录了的数据集保存在 `data/` 和 `inst/extdata/` 中。

使用到的方法以及其他类似或相关的项目和包件，也应该被记录标注。

如果数据结构不同于类似的软件包，\[*Bioconductor* reviewers\]\[reviewer-list\] 将期望一些理由说明原因。 请记住，扩展现有的类（class）总是可能的。

## Vignettes

Vignettes用于演示如何使用包的核心功能完成重要任务。 有两种常见类型的vignettes。

-   *Sweave* vergnette是一个 `.Rnw` 文件，它包含 $\\LaTeX$ 和一些 <i
    class="fab fa-r-project"></i> 代码。 代码块以`<<>>=`开始，以`@`结束。 在`R CMD build`期间，每个代码块都会被评估。之后 $\LaTeX$会被编译为一个PDF文档。

-   *R Markdown* vergnette 类似于 *Sweave* vergnete, 但文本部分使用[Markdown](http://daringfireball.net/projects/markdown/) 而不是$\\LaTeX$，并最终生成HTML输出。 `r BiocStyle:::CRANpkg("knitr")` 软件包可以处理大多数 *Sweave* 和所有 *R markdown* vignettes，生成美观的输出。 请参阅\[Writing package vignettes\]\[CRAN vigs\] 获取技术细节。 请参阅 `r BiocStyle::Biocpkg("BiocStyle")`包获取常用宏和标准 *Bioconductor* 风格的vignette。

Vignette提供了可重复性：vignette将相应的命令复制到一个<i class="fab fa-r-project"></i>session，生成相同的 结果。 因此，嵌入可执行<i
class="fab fa-r-project"></i> 代码是 **不可缺少的**。 快捷键(例如，使用 $\\LaTeX$ 逐字环境（verbatim environment），或使用 *Sweave* `eval=FALSE` ， 或 其他类似的Markdown技巧）有损于vignettes的功用， 一般不被允许 ****; 除非有适当的 理由，并由\[*Bioconductor* reviewers\]\[reviewer-list\]决定。

所有软件包都必须至少有一个 Rmd 或 Rnw vignette。 Vignettes 在包的 `vignetes/` 目录中。 Vignettes 一般是个独立的文件，所以最佳做法是包含一个有信息量的标题，主要作者，最后修改日期，和一个包的网址链接。 我们鼓励使用 `r BiocStyle::Biocpkg("BiocStyle")`来格式，将 `html_document` 作为输出目标。 vignette里放上下列 将会完成上述建议：

    output:
      BiocStyle::html_document:
        toc: true
        toc_depth: 2

编写 *Bioconductor* vignettes 的一些最佳做法和要求详见以下章节。

### 介绍

Add an “Introduction” section that serves as an abstract to introduce the objective, models, unique functions, key points, etc that distinguish the package from other packages in the same area. This is a requirement of Bioconductor package vignettes. It should include a short motivation for the package in general as well as motivation for inclusion of the package in Biconductor. When relevant, a brief review and comparison of packages with similar functionality or scope should be provided either in the Introduction or in a separate dedicated vignette section.

### 安装

添加一个“安装”部分，向用户显示如何下载和 加载Bioconductor包.

These instructions and any installations instructions should be in an `eval=FALSE` code chunk. No where in the code (<i class="fab fa-r-project"></i> code, man pages, vignettes, Rmd files) should someone try to install or download system dependencies, applications, packages, etc. Developers can provide instructions to follow in unevaluated code chunks, and should assume all necessary dependencies, applications or packages are already set up on a user’s system.

### 目录

在适当情况下，我们强烈鼓励包含一个目录。

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
