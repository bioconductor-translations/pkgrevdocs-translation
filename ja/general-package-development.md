# 一般 *バイオ伝導体* パッケージ開発

## *バイオ伝導体* およびformat@@2 <i class="fab fa-r-project"></i>

パッケージ開発者は、 *Bioconductor*\]\['devel' version\] と \[*Bioconductor* packages\]\[biocViews\] を常に使用し、 に貢献するパッケージを開発しテストする必要があります。

Depending on the <i class="fab fa-r-project"></i> release cycle, using \[*Bioconductor*\]\[\] devel may or may not involve also using the devel version of <i class="fab fa-r-project"></i>. 最新の情報については、 \[using devel version of Bioconductor\]\[‘devel’ version\] を参照してください。

## Correctness, Space and Time

### R CMD build

\[*Bioconductor*\]\[\] のパッケージは少なくとも `R CMD build`  (もしくは `R CMD INSTALL --build`) をパスしないといけません。 また `R CMD check` を error と warning 無しでパスしないといけません。そしてそれは最近の R-devel 版を使ってでないといけません。 作成者は、ビルドまたはチェック中に発生するすべての error 、 warning 、および note に対処しようとする必要があります。

### BiocCheck

パッケージは `BiocCheck::BiocCheckGitClone()` と `BiocCheck::BiocCheck('new-package'=TRUE)` を error と warning 無しでパスしなければいけません。 *[BiocCheck](https://bioconductor.org/packages/3.15/BiocCheck)* パッケージ は \[*Bioconductor*\]\[\] のベストプラクティスを含む一連のテストです。 パッケージ作成者は、ビルドまたはチェック中に発生するすべての error 、 warning 、および note に対処しようとする必要があります。

### ERROR 、 WARNGING 、 と NOTES

The \[*Bioconductor*\]\[\] team member assigned to review the package during the submission process will expect all ERROR, WARNINGS, and NOTES to be addressed from both R CMD build, R CMD check, and BiocCheck. If there are any remaining, a justification of why they are not corrected will be expected.

### File names

すべてのファイルシステムが大文字と小文字を区別するわけではないため、OS 等の場合に限って扱いが異なるファイル名は使用しないでください。

### Package size

`R CMD build` の実行から得られるソースパッケージは、ディスク上で 5 MB 未満である必要があります。

### Check duration

パッケージは、 10 分未満で `R CMD check --no-build-vignettes` を実行する必要があります。 `--no-build-vignettes` オプションを使用すると、vignetteが一度だけビルドされるようになります。 [1]

### Memory

Vignette と man ページ中の examples は 3 GB より大きいメモリを使ってはいけません。 32-bit のウインドウズでは <i class="fab fa-r-project"></i> はこれ以上のメモリをアロケートできないからです。

### Individual file size

For software packages, individual files must be &lt;= 5MB. This restriction exists even after the package is accepted and added to the \[*Bioconductor*\]\[\] repository. See [data section](#data) for advice on packages using large data files.

### Undesirable files

The raw package directory should not contain unnecessary files, system files, or hidden files such as `.DS_Store`, `.project`, `.git`, cache files, log files, `*.Rproj`, `*.so`, etc. These files may be present in your local directory but should not be commited to git (see <span
id="gitignore">`.gitignore`</span>). Any files or directories for other applications (Github Actions, devtool, etc) should ideally be in a different branch and not submitted to the *Bioconductor* version of the package.

## R CMD check environment

It is possible to activate or deactivate a number of options in `R CMD build` and `R CMD check`. Options can be set as individual environment variables or they can be [listed in a file](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Checking-and-building-packages). Descriptions of all the different options available can be found [here](https://cran.r-project.org/doc/manuals/r-devel/R-ints.html#Tools). \[*Bioconductor*\]\[\] has chosen to customize some of these options for incoming submission during `R CMD check`. The file of utilized flags can be downloaded from [<i class="fab fa-github"></i>
GitHub](https://github.com/Bioconductor/packagebuilder/blob/master/check.Renviron). The file can either be placed in a default directory as directed [here](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Checking-and-building-packages) or can be set through environment variable `R_CHECK_ENVIRON` with a command similar to:

    export R_CHECK_ENVIRON = <path to downloaded file>

[1] This is true only for Software Packages. Experiment Data, Annotation, and Workflow packages are allowed additional space and check time.
