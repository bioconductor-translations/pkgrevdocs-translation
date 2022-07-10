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

在舊函數的手冊頁 (man page) 中，指出該舊函數已棄用，並建議一個替換函數。 請確保在小插圖代碼塊 ( vignette code chunks) 或手冊頁 (man page examples) 示例中未調用舊函數； 執行 R CMD 檢查時需提供如下報告。

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

### 步驟 2 ：將函數標記為已失效

在函數被棄用後，請確保所有該棄用函數，在下一個發布週期的開發分支版本，已經無法運行且失效。 這意味著調用舊函數時，將不會運行任何其他代碼，並且返回一個信息性錯誤。 範例：

    myOldFunc <- function()
    {
        .Defunct("myNewFunc")
    }

有關詳細信息，請參閱 <code>?Defunct</code>。

刪除已棄用函數 (defunct function) 的文檔，並添如下代碼到手冊頁：

    \name{MyPkg-defunct}
    \alias{myOldFunc}
    \title{Defunct functions in package \sQuote{pkg}}
    \description{These functions are defunct and no longer available.}
    
    \details{
      Defunct functions are: \code{myOldFunc}
    }

### 步驟 3：刪除已棄用函數

在下一個發布週期中，在您的功能被標記為 defunct，從你的包 R 代碼和 devel 分支中的 NAMESPACE 中完全刪除它。 同時，刪除該功能的任何手冊頁內容。

保留上一步的手冊頁，以便調用下面函數時

    help("MyPkg-defunct")

仍然顯示已失效函數的列表及其相應的替代函數。

## 如何棄用包數據集

### 步驟 1 - 保存一個承諾對象 (a promise object)

棄用數據集的第一步是向 Bioconductor 的開發分支所有使用者發出警告訊息，表明該數據集將不再被使用。 可以使用 `warning` 消息，在開發者加載該數據集時進行提示該數據已棄用。 我們首先使用 `delayedAssign` 函數，創建一個承諾對象 (a promise object) 與棄用數據集名稱同名．請使用以下範例來完成上述步驟：

    delayedAssign(
        x = "pkgDataset",
        value = {
            warning("'pkgDataset' dataset is deprecated; see '?newData'")
            pkgDataset
        }
    )

您還包含輸出該棄用數據集在警告提示訊息裏。 然後，我們使用 `save` 函數將包中的原始 `.Rda` 數據集文件替換為 promise 對象和數據集：

    save("pkgDataset", eval.promises = FALSE, file = "data/pkgDataset.Rda")

將 `eval.promises` 參數設置為 `FALSE`，我們可以延遲 評估承諾對象，直到用戶加載數據 `data("pkgDataset", package = "yourPkg")`。 然後用戶會得到數據集與警告訊息。 警告訊息應包括說明這會將用戶指向一個新的數據集．如果有必要，也可提供函數去替換數據集．

### 步驟 2 - 更新文檔

保存承諾對像後，我們會更新文檔以反映更改，並根據情況為用戶提供額外的詳細信息和資源。 我們建議在數據文檔標題中包含“\[Deprecated\]”標籤。

### 步驟 3 - 取消數據集

In the following release cycle, you can update the warning message to indicate that the dataset is defunct and remove it entirely from the promise object i.e., from the expression in the `delayedAssign` function. We can also update the “\[Deprecated\]” label in the documentation title to “\[Defunct\]”.

## 如何棄用包

請參閱 [Package End of Life Policy](#package-end-of-life-policy)。
