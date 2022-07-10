# 棄用指南 （Deprecation Guidelines）

在軟件開發的過程中，可能需要刪除某些函數 (function)、類 (class)、方法 (methods) 或數據對象 (data objects)。 以下是一些指導方針，在刪除上述項目過程中，可確保對使用者造成的最小程度的干擾。

## 什麼需要棄用 (Deprecate)？

任何不再使用或不再需要的函數、類、方法或數據。

## 何時遵循棄用準則？

如果你在包的 devel 分支版本中引入一個函數，決定不再使用，您不遵循這些準則，並且直接刪除該函數。 可預計 devel 分支版本是不穩定的，並且會在沒有通知的情況下更改 API（儘管您可通過 Bioconductor \[support site\]\[\] 將這些更改通知您的用戶）。

但是，如果有一個函數至少存在於一個已發布的 Bioconductor 版本中 ，那*必須*遵守這些準則。 刪除函數、類、方法或任何導出的包對象的過程，大約需要三個發布週期（約 18 個月）。

## 如何棄用函數

### 步驟１：棄用該功能

當你開始決定移除一個函數時，你應該在開發分支版本，把它標記為已棄用。 可以通過調用 <code>.Deprecated()</code> 內部函數，來標記棄用函數。 為此，您必須提供一個替換函數，應該用來代替舊的函數。 範例：

    myOldFunc <- function()
    {
        .Deprecated("myNewFunc")
        ## 使用新函數, 或在此提供舊函數 (myOldFunc) 的提示
    }

這會在用戶調用 <code>myOldFunc()</code> 時，發出警告提示。 請參閱 <code>?.Deprecated</code> 了解更多資訊。

在舊函數的手冊頁 (man page) 中，指出該舊函數已棄用，並建議一個替換函數。 Be sure the old function is not called in man page examples or vignette code chunks; R CMD check should report this.

    \name{MyPkg-deprecated}
    \alias{MyPkg-deprecated}
    \title{Deprecated functions in package \sQuote{MyPkg}}
    
    \description{
      These functions are provided for compatibility with older versions
      of \sQuote{MyPkg} only, and will be defunct at the next release.
    }
    
    \details{
    
      The following functions are deprecated and will be made defunct; use
      the replacement indicated below:
      \itemize{
    
        \item{myOldFunc: \code{\link{newFunc}}}
    
      }
    }

### Step 2: Mark the function as defunct

In the next release cycle, after your function has been deprecated, it must be made defunct in the devel branch. This means a call to the old function will return an informative error but not run any additional code. Example:

    myOldFunc <- function()
    {
        .Defunct("myNewFunc")
    }

See <code>?Defunct</code> for more information.

Remove the documentation of the defunct function, and add to a man page such as the following:

    \name{MyPkg-defunct}
    \alias{myOldFunc}
    \title{Defunct functions in package \sQuote{pkg}}
    \description{These functions are defunct and no longer available.}
    
    \details{
      Defunct functions are: \code{myOldFunc}
    }

### Step 3: Remove the function

In the next release cycle, after your function has been marked as defunct, remove it entirely from your package R code and NAMESPACE in the devel branch. Also remove any man page content that documents the function.

Leave the man page from the previous step in place so that

    help("MyPkg-defunct")

still shows the list of defunct functions and their appropriate replacements.

## How To Deprecate A Package Dataset

### Step 1 - Save a promise object

The initial step of deprecating a dataset is to signal to any users on the devel branch of Bioconductor that the dataset will no longer be used. This can be done using a `warning` message when the devel user loads the dataset. In order to do this, we first create a promise object with the same name as the dataset name using the `delayedAssign` function:

    delayedAssign(
        x = "pkgDataset",
        value = {
            warning("'pkgDataset' dataset is deprecated; see '?newData'")
            pkgDataset
        }
    )

You can also include the dataset as an output after the warning. We then replace the original `.Rda` dataset file in the package with the promise object and dataset using the `save` function:

    save("pkgDataset", eval.promises = FALSE, file = "data/pkgDataset.Rda")

With the `eval.promises` argument set to `FALSE`, we can delay the evaluation of the promise object until the user loads the data with `data("pkgDataset", package = "yourPkg")`. The user will then get a warning along with the dataset. The warning should include instructions that will point the user to a new dataset or functionality that will replace the data, if necessary.

### Step 2 - Update documentation

After the promise object has been saved, we update the documentation to reflect the changes and provide additional details and resources for users as necessary. It is recommended to include a “\[Deprecated\]” label in the data documentation title.

### Step 3 - Defunct the dataset

In the following release cycle, you can update the warning message to indicate that the dataset is defunct and remove it entirely from the promise object i.e., from the expression in the `delayedAssign` function. We can also update the “\[Deprecated\]” label in the documentation title to “\[Defunct\]”.

## How to Deprecate a Package

Please see section on [Package End of Life Policy](#package-end-of-life-policy)
