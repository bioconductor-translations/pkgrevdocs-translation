# `.gitignore` 檔案設定

\[*Bioconductor*\]\[\] 需要 Git 存儲庫 (repository) 才能提交。

某些系統文件不應被包含在版本追蹤，且需要排除在存儲庫 。 建議使用 `.gitignore` 文件來可以保留在本地系統，並且從 Git 存儲庫中排除．

以下是 *[BiocCheck](https://bioconductor.org/packages/3.15/BiocCheck)* 檢查的文件 和標記為不可接受的檔案類別：

    hidden_file_ext = c(
        ".renviron", ".rprofile", ".rproj", ".rproj.user", ".rhistory",
        ".rapp.history", ".o", ".sl", ".so", ".dylib", ".a", ".dll",
        ".def", ".ds_store", "unsrturl.bst", ".log", ".aux", ".backups",
        ".cproject", ".directory", ".dropbox", ".exrc", ".gdb.history",
        ".gitattributes", ".gitmodules", ".hgtags", ".project", ".seed",
        ".settings", ".tm_properties"
    )

請使用一個 `.gitignore` 檔案並且存放在每個包的頂層目錄。
