# Linux 基礎

## 00. 終端機基本用法

### 文件或目錄顏色
* 白色 - 普通文件
* 綠色 - 可執行文件
* 紅色 - 壓縮文件
* 藍色 - 目錄
* 青色 - 連結文件 link file
* 黃色 - 設備文件: 塊block, 字符char, 管道fifo
* 灰色 - 其他文件

### 命令參數

* 大部分命令參數前有`-` (System V風格)
```
ls -a
rm -r dir
```
* 有些有沒有橫線都可以 (BSD風格)
```
tar xvf filename
tar -xvf filename
ps aux
ps -aux
```
* 一般格式

    * 單橫線 + 字母 [ `-r | -p | -l`]
    * 雙橫線 + 單字 [ `--help | --version` ]

## 01. Shell操作

基本概念：shell解析被輸入到終端機的命令，按順序搜索**環境變量($PATH)**中的路徑，找到輸入的命令後運行該程序，在終端機中有對應的輸出

1. 命令或目標補齊
    * 快捷鍵: `TAB`

2. 遍歷歷史紀錄
    * `history`: 顯示過去使用過的命令
    * `Ctrl + P | UP`: 顯示前一行執行過的命令
    * `Ctrl + N | DOWN`: 顯示後一行執行過的命令
3. 光標移動
    * `Ctrl + B | LEFT`: 往左移動
    * `Ctrl + F | RIGHT`: 往右移動
    * `Ctrl + A`: 移動到頭部
    * `Ctrl + E`: 移動到尾部
4. 刪除字符
    * `Ctrl + H | BACKSPACE`: 退格，刪除*光標之前*的字符
    * `Ctrl + D | DELETE`: 刪除*光標後面(光標蓋住)*的字符
    * `Ctrl + U`: 刪除光標之前的所有字符串，若光標在最後，就是*整行刪除*
    * `Ctrl + K`：刪除光標之後的所有字符串(包含光標蓋住的)

### 練習題
* 快捷鍵
    1. 遍歷歷史紀錄？
    2. 向上？
    3. 向下？
    4. 快速完成補全命令/路徑？
* 刪除
    1. 刪除光標前字符？
    2. 刪除光標後字符？
    3. 刪除光標前字符串？
    4. 刪除光標前字符串？
* 移動
    1. 向前？
    2. 向後？
    3. 移動到頭部？
    4. 移動到尾部？

## 02. Linux系統目錄結構

### 根目錄表示方式 
`/`

### 根目錄下常見目錄
* `/bin`: binary，二進制文件、可執行程序(綠色 `date` `ls`，青色)、shell命令

    Q: 為什麼執行`ls`，檔案顯示有高亮，執行`/bin/ls`沒有？  
    A: 執行`alias`，可以發現  
    ```
    alias ls='ls --color=auto
    ```

* `/dev`: device，在Linux下一切皆文件，硬體(硬碟、顯示卡、顯示器)也被抽象為文件
* `/lib`: library，Linux運行時需要加載的動態庫，相當於Windows的dll  
    `.so`表示動態庫，後面的數字表示版本號
* `/mnt`: mount，手動的掛載目錄
* `/media`: media，外設的自動掛載目錄
* `/root`: root，Linux的超級用戶的家目錄
* `/usr`: unix system resource，資源目錄
    * `/usr/include` - Header files，裡面有`stdio.h`, `stdlib.h`
    * 遊戲
    * `/usr/local` - 用戶安裝的應用程序
* `/etc`: 配置文件
    * `/etc/passwd` - 當前Linux系統的用戶訊息
    * `/etc/group` - 當前Linux系統的用戶組訊息
    * `man 5 passwd`
* `/opt`: 安裝第三方應用程序
* `/home`: Linux 操作系統所有用戶的家目錄
    * 用戶家目錄(宿主目錄)：如`/home/crossluna`
* `/tmp`: 存放臨時文件，重新啟動電腦會清空

## 03. 路徑表示
1. 相對路徑:從當前的目錄開始表示  
    ```
    ./zoo/animal/food
    ```

2. 絕對路徑
    從根目錄`/` 開始表示
    ```
    /home/kevin/demo/1Day/zoo/animal/food
    ```
3. `.` 和 `..`
    * `.` : 當前目錄
    * `..` : 當前目錄的上一層目錄
4. `kevin@ubuntu:~/demo/1Day$` 是什麼意思?
    * `kevin`: 當前登錄的用戶
    * `@`: at，在
    * `ubuntu`: 安裝時指定的主機名稱
    * `~`: 用戶的家目錄(宿主目錄)
    * `~/demo/1Day`: 當前用戶的工作目錄
    * `$`: 表示當前用戶是普通用戶
    * `#`: 表示當前用戶是超級用戶

## 04. 文件目錄相關命令

### `tree`
* 查看目錄的內容，並畫出樹狀圖
* `tree`: 查看當前目錄
* `tree dir`: 查看指定目錄
* 需要額外安裝: `sudo apt-get install tree`

### `ls`
* 功能: 查看文件或目錄
* 語法:
* 參數:
    + `-a` 顯示所有文件
    + 隱藏文件: 文件或目錄名前面有一個點`.`
    + `-h` human，用人類看得懂的方式顯示
    + `-l` list，顯示詳細訊息
    + `-rwxrw-r-- 1 kevin kevin 3231145 Nov 23 23:08 vimplus.tar.gz`  
    + 第一個字符：文件的類型，共有7種  
        `-`: 普通文件 (`.txt`、壓縮包、可執行程序都是)  
        `d`: 目錄  
        `l`: 符號連結  
        `p`: 管道  
        `s`: 套接字 socket  
        `c`: 字符設備 char(鍵盤、滑鼠)  
        `b`: 塊設備 block(USB、硬碟)  
    + `rwxrw-r--`: 讀寫權限，分別代表 user所有者(`rwx` 可讀可寫可執行), group所屬組用戶(`rw-` 可讀可寫), other其他人(`r--` 唯讀)
    + `1`: 硬連接計數 hardlink
    + `kevin`(第一個): 文件所有者
    + `kevin`(第二個): 文件所屬群組
    + `3231145`: 文件的大小，若是目錄則一律`4096`
    + `Nov 23 23:08`: 日期
    + `vimplus.tar.gz`: 文件名
    + `-F` 如果是目錄，後面加一個`/`
    + `alias ll='ls -alF'`

### `cd`
* 切換目錄
* 如何進入到家目錄？
    + `cd /home/kevin`
    + `cd ~`
    + `cd`
* 在鄰近的兩個目錄(最行兩個)之間切換，適用於兩個目錄很長的情況
    + `cd -`
### `pwd`

    當前路徑 print working directory
### `mkdir`
* 創建目錄 `mkdir dir`
* 參數
    + `-p`: 創建多級目錄
        `mkdir -p aa/bb/cc`
### `touch`
* `touch filename`: 如果文件不存在，創建文件，若存在，更新文件的時間

### `rmdir`
* 刪除目錄，但僅能刪除空目錄
### `rm`
* 刪除文件或目錄
    `rm -r dir`
    `rm filename`
* 參數
    + `-r`: 遞迴刪除目錄(不管是否為空都要加)
    + `-i`: 刪除的時候提示
* 注意: 刪除之後，很難恢復
### `cp`
* 拷貝文件
    + `cp sourcefile not_existing_file`: 自動創建`not_existing_file`並把`sourcefile`內容拷貝
    + `cp sourcefile existing_file`: 覆蓋`existing_file`內容
    + `cp sourcefile dir`: 拷貝並放到`dir`下
    + `cp sourcedir desdir -r`: 把`sourcedir`整個拷貝到`desdir`底下(需要加上參數`-r`)
    + `cp sourcedir not_existing_dir -r`: 創建 `not_existing_dir` 並把`sourcedir`中的內容拷貝到`not_existing_dir`中，不包括`sourcedir`(注意不同於路徑已經存在的狀況)
### `mv`
* 改名或移動文件 `mv file1 file2`
* 改名
    + `mv existing_file not_existing_file`: 把檔案改名成`not_existing_file`
    + `mv existing_dir not_existing_dir`: 改名目錄
* 移動
    + `mv file existing_dir`: 把文件移動到目錄中
    + `mv existing_dir1 existing_dir2`: 把`dir1`移到`dir2`底下
* 極端情況，發生覆蓋
    + `mv existing_file1 existing_file2`: `file1`將會覆蓋`file2`

### 查看文件內容
#### `cat`
* `cat filename`: 把文件內容打印到終端機，適合用於內容比較少時
#### `more`
* `more filename`: 可以向下捲動，按`ENTER`向下瀏覽一行，`SPACE`可以往下捲一頁，無法往上捲，按`q`退出
#### `less`
* `less filename`  
    + 向下滾動一行: `ENTER` `CTRL+N`
    + 向上滾動一行: `CTRL+P`
    + 向下翻頁: `SPACE` `PGDN`
    + 向上翻頁: `PGup`
    + 推出: `q`
#### `head` `tail`
* 僅顯示頭部或尾部，默認10行，可以用`-5`改成5行

### 軟硬連接 `ln`
link，類似於Windows下的捷徑
* 軟連接 - 快捷方式
    + `ln -s filename(absolte dir) name_of_link`
    + 例:  
    ```
    $ ln -s ~/linux_programming/programmer h.soft
    $ ll
    lrwxrwxrwx   1 crossluna crossluna   10 Mar 16 15:55 s.soft -> programmer*
    $ ./h.soft
    Programmer!
    $ mv h.soft ..
    $ ./../h.soft
    Programmer!
    ```
    + 目錄也可以創建軟連接

* 硬連接
    + 相當於取了一個別名
    + Linux是利用inode來定義文件，找到數據塊，硬連接取了個別名指到同一個inode，不佔用磁碟空間
    + `ln filename(absolute or relative)  name_of_link`
    + 例:  
    ```
    $ ln programmer h.hard
    $ ll
    -rwxrwxr-x   2 crossluna crossluna 9224 Mar 16 15:54 h.hard*
    $ rm programmer
    $ ll
    -rwxrwxr-x   1 crossluna crossluna 9224 Mar 16 15:54 h.hard*
    ```
    + 用途
        1. 創建一個新文件，硬連接計數為1
        2. 給文件創建了硬連接，硬連接計數為2
        3. 刪除一個硬連接，硬連接計數為1
        4. 再刪除硬連接計數對應的文件，硬連接計數為0，成為廢棄的數據塊，OS就不再保護這一塊，也就是說，刪除文件只是解放了連接。
    + 使用場景
        - 磁碟上有個文件 `/home/kevin/hello`
        - 在其他多個目錄中管理`hello`，並且能時時同步編輯

## 05. 用戶權限、用戶和用戶組
### 修改文件或目錄權限 `chmod`
* 文字設定法 `chmod who [+|-|=]mode filename`
    - who
        + `u` - user 文件所有者
        + `g` - group 文件所屬組
        + `o` - other 其他人
        + `a` - all 所有人(默認)
    - `+|-|=` 增加|減少|覆蓋
    - mode
        + `r`: 讀
        + `w`: 寫
        + `x`: 執行
        + `-`: 沒有任何權限
    - **目錄必須有執行權限，否則進不去**
    - 練習題 `rwxrwxrwx -- file`
        1. 文件所有者和其他人減去讀寫權限  
            `chmod uo-rw file`
        2. 所有者增加讀權限，同組用戶減去執行權限  
            `chmod u+r,g-x file`
        3. 所有人減去權限  
            `chmod a-w file`  

* 數字設定法 `chmod [+|-|=]mode filename`
    - mode，一個八進制的數
        + `4`: r
        + `2`: w
        + `1`: x
        + `0`: -
    - 練習題 `--xrwx--x file`
        1. 所有者和同組用戶的權限設置為`-wx`，其他只有權限1  
            `chmod 331 file`
        2. 文件權限`777`，給所有者和所屬組減去`r`  
            `chmod -440 file`
* 注意
    ```
    -rwxrwxrwx file
    $chmod -w file
    chmod file: 新權限為 r-xr-xrwx，非 r-xr-xr-x
    ```
    默認保護機制，如果真的要照此操作，使用  
    `chmod a-w file`
    
### 修改文件所有者或所屬群組 `chown`
* `chown new_owner filename`
* `chown new_owner:new_group filename`
* 怎麼找有哪些用戶，哪些組? `/etc/passwd` `/etc/group`
* 例子  
    ```
    $ cat /etc/passwd
    $ cat /etc/group
    $ chown robin file
    chown: 正在更改'file'的所有者，不允許的操作
    $ sudo chown robin file
    $ sudo chown kevin:luffy file 
    ```
### 修改文件所屬組 `chgrp`
* `chgrp new_group filename`  
    ```
    $ sudo chgrp kevin file
    ```

## 06. 文件查找與檢索
### 根據文件屬性查找 `find`
* 文件名  
    `find dir -name "filename"`
    為什麼需要引號? 表達式的通配符，escape character
* 文件類型  
    `find dir -type filetype`
        - **普通文件** `f`
        - 目錄 `d`
        - 符號連接 `l`
        - 管道 `p`
        - 套接字 `s`
        - 字符設備 `c`
        - 塊設備 `b`
* 文件大小  
    `find dir -size +10k` 查找大於10k的文檔
        - `+10k` 大於10k
        - `-10k` 小於10k
        - `10k` 等於10k
        - 單位: `k`小寫， `M`大寫
        - 大於10k小於100k `-size +10k -size -100k`
* 按日期  
    - 創建日期 `-ctime [-n/+n]`
        + `-n` n天以內
        + `+n` n天以外
        + `find dir -ctime -1`
    - 修改日期 `-mtime [-n/+n]`
    - 訪問日期 `-atime [-n/+n]`
* 深度
    + `find dir -maxdepth n(number of depth)`
    + `find dir -mindepth n`
    + 層數從`dir`開始算1
* 高級查找
### 根據文件內容查找 `grep`
* `grep -r(有目錄) "查找的內容" 搜索的路徑`  
* 例子:搜索家目錄中帶有helloworld的文件 `grep -r "helloworld" ~`
* `-n`: 搜索結果，加上行數

### 總結
* `find dir param content`
* `grep content param dir`，注意跟`find`順序不同

## 07. 壓縮包管理
### Linux下常見的壓縮格式
* `.gz` - `gzip`壓出來的
* `.bz2` - `bzip2`壓出來的  
然而`gzip`和`bzip2`不太好用，沒有打包功能

### 常用壓縮命令
#### `tar`
打包工具
* 參數
    + `c` 創建壓縮文件
    + `x` 釋放壓縮文件(解壓縮)
    + `v` verbose，打印提示
    + `f` 指定壓縮包的名字
    + `z` 使用gzip的方式壓縮文件，生成`xxx.tar.gz`
    + `j` 使用bzip2的方式壓縮文件，生成`xxx.tar.bz2`
* 用法   
    `tar [params] [pkg_name] [orig_file1 orig_file2 ...]`  
    `tar [params] [pkg_name] -C [obj_dir]`  
    + 壓縮 `tar zcvf test.tar.gz file dir`
    + 解壓縮 `tar zxvf test.tar.gz -C temp/` 

#### `rar`
* 需要安裝 `sudo apt-get install rar`
* 壓縮 `rar a [pkg_name(w/o extension)] [orig_files]`
    - 如果有**目錄**，要加參數 `-r`
* 解壓縮 `rar x [pkg_name(w/o extension)] [obj_dir]`
#### `zip`/`unzip`
* 壓縮
    `zip [params] [pkg_name] [orig files]`
    - 如果有**目錄**，要加參數 `-r`
* 解壓縮
    `unzip [pkg_name] -d [obj_dir]`
### 總結
* 壓縮 `tar/rar/zip [params] [pkg_name] [orig_file1 orig_file2 ...]`
* 解壓縮 `tar/rar/unzip [params] [pkg_name] [-params] [obj_dir]`
    + `rar` 解壓縮到指定目錄不需要指定參數
    + `unzip` 不需要解壓參數

## 08. 軟件安裝與卸載
* 總是先跑 `sudo apt-get update` 更新來源訊息
* 安裝`sudo apt-get install [pkg_name1] [pkg_name2] ...`
* 卸載
    + 移除包裹但是保留組態configuration `sudo apt-get remove [pkg_name]`
    + 包裹和組態都移除 `sudo apt-get purge [pkg_name]`
        - 然而家目錄`~`下的組態一般不受影響
