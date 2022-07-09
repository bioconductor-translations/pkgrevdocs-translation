# 帮助文档

软件包文档对于用户理解如何使用你的代码非常重要。

## Bioconductor文档基本要求：

-   一个 \[vignette\]\[CRAN vigs\] 的Rmd 或 Rnw 格式文档，带有可执行的 代码，显示如何使用包来完成任务。

-   \[man pages\]\[CRAN Rd\] 记录所有导出的函数，并有它们的操作 示例，记录完善的数据结构，尤其是如果不是已经存在 \[pre-exiting class\]\[bioc-common\]的。

-   详细记录了的数据集保存在 `data/` 和 `inst/extdata/` 中。

使用到的方法以及其他类似或相关的项目和包件，应该被引用和标注。

如果数据结构不同于类似的软件包，\[*Bioconductor* reviewers\]\[reviewer-list\] 将期望一些理由说明原因。 请记住，扩展现有的类（class）总是可能的。

## Vignettes

Vignette用于演示如何使用包的核心功能完成重要任务。 有两种常见类型的vignette。

-   *Sweave* vergnette是一个 `.Rnw` 文件，它包含 $\\LaTeX$ 和一些 <i
    class="fab fa-r-project"></i> 代码。 代码块以`<<>>=`开始，以`@`结束。 在`R CMD build`期间，每个代码块都会被评估。之后 $\LaTeX$会被编译为一个PDF文档。

-   *R Markdown* vergnette 类似于 *Sweave* vergnete, 但文本部分使用[Markdown](http://daringfireball.net/projects/markdown/) 而不是$\\LaTeX$，并最终生成HTML输出。 `r BiocStyle:::CRANpkg("knitr")` 软件包可以处理大多数 *Sweave* 和所有 *R markdown* vignettes，生成美观的输出。 请参阅\[Writing package vignettes\]\[CRAN vigs\] 获取技术细节。 请参阅 `r BiocStyle::Biocpkg("BiocStyle")`包获取常用宏和标准 *Bioconductor* 风格的vignette。

Vignette提供了可重复性：vignette将相应的命令复制到一个<i class="fab fa-r-project"></i>session，生成相同的 结果。 因此，嵌入可执行<i
class="fab fa-r-project"></i> 代码是 **不可缺少的**。 快捷键(例如，使用 $\\LaTeX$ 逐字环境（verbatim environment），或使用 *Sweave* `eval=FALSE` ， 或 其他类似的Markdown技巧）有损于vignettes的功用， 一般不被允许 ****; 除非有适当的 理由，并由\[*Bioconductor* reviewers\]\[reviewer-list\]决定。

所有软件包都必须至少有一个 Rmd 或 Rnw vignette。 Vignette在包的 `vignetes/` 目录中。 Vignette一般是个独立的文件，所以最佳做法是包含一个有信息量的标题，主要作者，最后修改日期，和一个包的网址链接。 我们鼓励使用 `r BiocStyle::Biocpkg("BiocStyle")`来格式，将 `html_document` 作为输出目标。 vignette里放上下列 将会完成上述建议：

    output:
      BiocStyle::html_document:
        toc: true
        toc_depth: 2

编写 *Bioconductor* vignettes 的一些最佳做法和要求详见以下章节。

### 导言 Introduction

增加一个“导言”部分，作为一个摘要，介绍 目的、模型， 独有的函数、其他关键点等，用于 区分和同一领域其他包有什么不同。 这是Bioconductor vignette的要求。 它应该包含简短的本包的 动机以及为什么 Bioconductor 应该包含 此包的动机。 当相关时， 简短评论 和与其功能类似或范围比较接近的软件包应该在导言或单独一个专用vignette 部分中提供 。

### 安装 Installation

添加一个“安装”部分，向用户显示如何下载和加载Bioconductor包。

这些说明和任何安装说明应该在一个 `eval=FALSE` 代码块中。 文档 (<i class="fab fa-r-project"></i> code, man pages, vignettes, Rmd files) 不应该试图安装或下载系统依赖， 应用程序、 软件包等。 开发者可以在不被执行的代码块里为 提供指导，并应假设所有必需的 依赖、应用程序或软件包已经在用户的 系统上设置好。

### 目录 Table of contents

在适当情况下，我们强烈鼓励包含一个目录。

### 执行代码块 Evaluated code chunks

有意义的可执行的代码是必须的！！！

静态vignettes是不可接受的。

### 会话信息 Session information

在vignette末尾包含一个 `SessionInfo()` 的部分。

### `vignettes/` 目录和中间文件

只有源vignette文件(`.Rnw` or `.Rmd`) 和任何必需的 静态图像应该在 vignette 目录中。 不应该存在中间文件。 这也包括完整的执行vignette后的产物；vignette应通过 软件包的 `R CMD build` 创建。 若要包含其他类型的文档，请使用 `instal/doc` 或其他恰当命名的 `inst` 目录。

### 参考文献 References

请记住包含任何有关方法的引用。

## Man 页面

请参阅\[Writing R Extensions section on man pages\]\[CRAN Rd\] 获取 详细的包的格式信息、 功能、类和数据集。

所有帮助页面都应该是全面的。

### 包一级文档

\[*Bioconductor*\]\[\] 鼓励有一个带有 软件包概览和与主要函数链接的软件包 man 页面。 用户可以用 `?<package name>`显示页面。

### 函数和类

所有导出的函数和类都需要有一个 man 页面。 描述新的类的man页面 必须非常详细地说明所储存的结构和信息的 类型。

### 数据

数据man页面必须包含来源信息和数据结构 信息。

### 示例

所有man页面都应该有一个可运行的例子。

`destest` 和 `dontrun` 不被鼓励使用且一般不被允许 ； 除非有适当的理由，并由\[*Bioconductor* reviewers\]\[reviewer-list\] 酌处。

如果执意使用此选项，则最好使用 `dontest` 而不是 `dontrun`; `dontest` 需要有效的
<i class="fab fa-r-project"></i> 代码，而 `dontrun` 不用。

## `inst/script/` 目录

此目录中的脚本可以有差异。

最重要的是，如果有数据包含在 `inst/extdata/` 目录中，那么， 必须有相关脚本存在于这个目录中，并 清楚地说明数据是如何生成的和源信息。

它应该包含源 URL 和任何其 过滤或处理的关键信息。

它可以是可执行代码，sudo代码或文本描述。

用户应该能够下载并能够大致复现存在于数据中的 文件或对象。

## 其它

其他类型的文档(例如静态文件、jupyter notebooks 等) 可以通过 `inst` 子目录提供，但不 能替代上面列出的 [*Bioconductor* 文档 要求](#doc-require)。
