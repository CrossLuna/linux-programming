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
    + `1`: 硬連接技術 hardlink
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

### 軟硬鏈接