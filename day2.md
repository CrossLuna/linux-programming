# Linux開發工具

## 00. 軟件的安裝與卸載

### 在線安裝 `apt-get`
* 安裝 `sudo apt-get(apt) install [pkg_name]`
* 卸載 `sudo apt-get remove [software_name]`
* 軟件列表的更新 `sudo apt-get update`
* 清空緩存(安裝包) `sudo apt-get clean`
    - `/var/cache/apt/archiv`
### 軟件包安裝 (Ubuntu底下 `.deb`格式)
* 安裝 `sudo dpkg -i xxx.deb`
* 卸載 `sudo dpkg -r [software_name]`

### 源碼安裝

## 01. `vim`相關
`vim`需要安裝，自帶教學 `vimtutor`

### `vim` 的三種工作模式
* 命令模式 Command mode
* 編輯模式 Insert mode
* 末行模式 Last line mode

### `vim`命令模式下的相關操作
#### 保存退出 `ZZ`
#### 代碼格式化 `gg=G`
#### 光標移動
    - 上下左右 `kjhl`
    - 行首 `0`
    - 行尾 `$`
    - 文件首 `gg`
    - 文件尾 `G`
    - 行跳到123行 `123G`
    - 當前行向下移動200行 `200 ENTER`

#### 刪除命令
* 刪除字符- 光標前的`X`，光標後的`x`
* 刪除單詞`dw`，刪除整個單詞光標應該要在單詞最前面
* 刪除光標前半行字符串 `d0`
* 刪除光標後半行字符串 `d$` `D`
* 刪除光標所在行 `dd`
* 刪除多行 `ndd`，`n`是行數
* 刪除整個文檔 `0dG`

#### 撤銷和反撤銷
* 撤銷 `u`
* 反撤銷 `CTRL+r`

#### 複製和貼上
* 複製一行`yy`，多行`nyy`
* 貼上
    - `p` 貼到光標的下一行
    - `P` 貼到光標的上一行
#### 可視模式 visual mode `v`
* 移動光標 `hjkl`
* 複製 `y`
* 刪除 `d`
* 貼上 `p`光標前面，`P`光標後面

#### 替換操作
* `r` 替換一個，光標蓋住的字符
* `R` 替換多個，從光標蓋住的往後替換

#### 查找命令
* `/xxx` 向下查找，`?xxx` 向上查找
* 關鍵字切換`n/N`，下一個/上一個
* `#` 光標移動到待搜索的關鍵字上面，鍵盤輸入`#`

#### 查看`man`文檔
* 命令行 `man [chapter] xxx`，例子`man 3 printf`
* 在`vim`裡面 `[chapter]+K`

### 命令模式切換文本編輯模式
* `a` 從光標後方插入，`A` 從行尾插入
* `i` 從光標前方插入，`I` 從行首插入
* `o` 從下一行插入，`O` 從上一行插入
* `s` 刪除光標上的字符並進入插入模式，`S` 刪除光標所在行並進入插入模式
* `ESC`回到命令模式
### `vim`末行模式下的操作
#### 從命令模式到末行模式 `:`
#### 保存退出 
* 保存不退出 `:w`
* 退出 `:q`
* 退出不保存 `:q!`
* 保存退出 `:wq = :x`，類似命令模式下的`ZZ`，但是會創建新文件
#### 行跳轉 `:n`
#### 末行模式回到命令模式
* 兩次`ESC`
* 在末行模式下執行完一個命令

#### 末行模式替換操作
* 替換光標所在行的字符串 `:s/[old]/[new]/gc`
    - `g`: 替換當前行所有的`[old]`
    - `c`: 替換的時候添加提示信息
* 替換一個範圍(22到28行) `:22,28s/[old]/[new]/g`
* 替換當前文檔所有的 `:%s/[old]/[new]/`

#### 分屏操作
* 當前文件分屏
    - 水平 `:sp`
    - 垂直 `:vsp`
* 兩個屏幕顯示不同的文件
    - 水平 `:sp [filename]`
    - 垂直 `:vsp [filename]`
* 屏幕的關閉
    - 關閉所有 `:qall`
    - 保存關閉所有 `:wqall`
    - 保存所有 `:wall`

* 屏幕的切換 `CTRL+ww`(兩次`W`)
* 打開的時候分屏
    - 水平 `vi -on [filename1 filename2 ...]` n表示分屏的個數，可以省略
    - 垂直 `vi -O [filename1 filename2 ...]`
#### 末行模式執行Shell命令 `:!ls` `:!pwd`

### `vim`配置文件
#### 用戶級別 `~/.vimrc`
#### 系統級別 `/etc/vim/vimrc`



## 02. `gcc`相關
### `gcc` 工作流程
* 預處理 `-E`
    - Macro替換
    - 展開header files
    - 去掉註釋
    - `xxx.c` -> `xxx.i`，`.i`還是一個C文件
* 編譯 `-S`
    - `xxx.i` -> `xxx.s`，是一個Assembly文件
* 彙編 `-c`
    - `xxx.s` -> `xxx.o`，是binary文件
* 連結 
    - `xxx.o` -> `xxx`，可執行bianry
### `gcc` 常用參數
* `-v --version` 查看版本
* `-I` 編譯時指定頭文件路徑
* `-c` 將Assembly文件生成binary文件，得到`.o`，可以隱藏源代碼
* `-o` 指定生成文件的名字
* `-g` `gdb`調試的時候需要加，會添加很多調試信息
* `-D` 在編譯時指定一個Macrio
    - 使用場景: 測試程序時 `-D DEBUG`
* `-Wall` 添加警告信息
* `-On` 優化代碼，`n`是優化級別，只有0123

## 03. 靜態庫和動態庫
### 庫是什麼?
* Binary文件
* 將源代碼 → Binary格式的源代碼
* 加密
### 庫製作出來之後，如何讓用戶使用?
* header files
* binary files 製作出來的庫
### 靜態庫的製作和使用
* 命名規則 `libxxx.a` 其中`xxx`是庫的名字
* 製作步驟 
    - 原材料`.c .cpp`
    - `gcc a.c b.c -c `將`.c`文件生成`.o`
    - `ar rcs libtest.a a.o b.o`，其中`ar`是個打包工具`archive`
    - `nm libtest.a` 印出symbols，確認生成無誤
* 庫的使用 `gcc main.c -I ./include/ -L ./lib/ -l mycalc -o app`
    - `-L` 指定庫的路徑
    - `-l` 指定庫的名字，去掉`lib`和`.a`
### 動態庫的製作和使用
* 命名規則 `libxxx.so`
* 製作步驟 
    - `gcc a.c b.c -c -fpic `將`.c`文件生成`.o`，記得加參數`-fpic`
    - `gcc -shared -o libxxx.so a.o b.o` 打包
* 庫的使用 `gcc main.c -I ./include/ -L ./lib/ -l mycalc -o app`
* 動態庫無法加載
    - ELF格式
    - `ldd app` 查看連結
    - 對於ELF格式的可執行程序，是由`ld-linux.so*`來完成的，他先後搜索ELF文件的`PT_RPATH`段 → 環境變量`LD_LIBRARY_PATH` → `/etc/ld.so.cache`文件列表 → `/lib/` `/usr/lib`目錄找到庫文件後載入內存 
    - 解法一：使用環境變量 `LD_LIBRARY_PATH`
        + 臨時設置 - 在terminal `export LD_LIBRARY_PATH=[dynamic library path]:$LD_LIBRARY_PATH`
        + 永久設置-用戶級別 把上面那行寫到`~/.bashrc`，再重啟終端或是執行`source ~/.bashrc`
        + 永久設置-系統級別 `/etc/profile`，重啟電腦或是 `source /etc/profile`
    - 解法二：更新`/etc/ld.so.cache`
        + 找到一個配置文件 `/etc/ld/so/conf`
        + 把動態庫的絕對路徑寫到裡面
        + 執行`sudo ldconfig -v`
* 知識點擴展 `dlopen` `dlclose` `dlsym`
