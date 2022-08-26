# C 和 Fortran 代碼

如果該 R 包包含 C 或是 Fortran 代碼，請遵照 \[Writing R Extensions\]\[\] 手冊裡 \[System and foreign language interfaces\]\[CRAN foreign\] 小節的標準與方法．

以下幾節中我們列出一些特別的注意事項。

### 内部函数

使用內部 <i class="fab fa-r-project"></i> 函數，例如 `R_alloc` 和隨機數生成器 (RNG)，而不是系統提供的函數。

### C 函數註冊

使用 C 函數註冊 (請參見 \[Writing R Extensions\]\[\] 手冊的 \[Registering native routines\]\[\] 小節)

### 檢查用戶中斷

當有可能因某些參數設置，或當他們的典型參數設置，運行時間超過 10 秒，並且 方法旨在用於交互式使用時，請在 C 級循環 （C level loops) 中使用 `R_CheckUserInterrupt()`．

### Makevars

建議靈活使用在每個包裡的 `Makevars` 和 `Makefile` 文件。 這些通常根本不需要（參見\[Writing R Extensions\]\[\] 手冊 的 \[Configure and cleanup\]\[CRAN config\] 小節）。

### 警告與最佳化 (Warnings and optimizations)

在包開發期間，啟用所有警告並禁用優化。 如果您打算[使用調試器](#debugging-cc-code)，請在編譯器設定您的調試符號。

執行這些的最簡單方法是創建用戶級 `Makevars` 文件用戶的主目錄在名為“.R”的子目錄中）。 查看下面範例 是常用工具鏈 (toolchains) 的標誌。 請查閱 \[Writing R Extensions\]\[\] 手冊的 \[Makevars files\]\[\] 取得更多細節．

Gcc/g++ 範例：

    CFLAGS=-Wall -Wextra -pedantic -O0 -ggdb
    CXXFLAGS=-Wall -Wextra -pedantic -O0 -ggdb
    FFLAGS=-Wall -Wextra -pedantic -O0 -ggdb

Clang/clang++ 範例:

    CFLAGS=-Weverything -O0 -g
    CXXFLAGS=-Weverything -O0 -g
    FFLAGS=-Wall -Wextra -pedantic -O0 -g
