# C 和 Fortran 代碼

如果該 R 包包含 C 或是 Fortran 代碼，請遵照 \[Writing R Extensions\]\[\] 手冊裡 \[System and foreign language interfaces\]\[CRAN foreign\] 小節的標準與方法．

以下幾節中我們列出一些特別的注意事項。

### 内部函数

使用內部 <i class="fab fa-r-project"></i> 函數，例如 `R_alloc` 和隨機數生成器 (RNG)，而不是系統提供的函數。

### C 函數註冊

使用 C 函數註冊 (請參見 \[Writing R Extensions\]\[\] 手冊的 \[Registering native routines\]\[\] 小節)

### 檢查用戶中斷

Use `R_CheckUserInterrupt()` in C level loops when there is a chance that they may not terminate for certain parameter settings or when their run time exceeds 10 seconds with typical parameter settings, and the method is intended for interactive use.

### Makevars

Make judicious use of the `Makevars` and `Makefile` files within a package. These are often not required at all (See the \[Configure and cleanup\]\[CRAN config\] section of the \[Writing R Extensions\]\[\] manual).

### Warnings and optimizations

During package development, enable all warnings and disable optimizations. If you plan to [use a debugger](#debugging-cc-code), tell the compiler to include debugging symbols.

The easiest way to enforce these is to create a user-level `Makevars` file user’s home directory in a sub-directory called ‘.R’). See examples below for flags for common toolchains. Consult the section about \[Makevars files\]\[\] in the \[Writing R Extensions\]\[\] manual.

Example for gcc/g++:

    CFLAGS=-Wall -Wextra -pedantic -O0 -ggdb
    CXXFLAGS=-Wall -Wextra -pedantic -O0 -ggdb
    FFLAGS=-Wall -Wextra -pedantic -O0 -ggdb

Example for clang/clang++:

    CFLAGS=-Weverything -O0 -g
    CXXFLAGS=-Weverything -O0 -g
    FFLAGS=-Wall -Wextra -pedantic -O0 -g
