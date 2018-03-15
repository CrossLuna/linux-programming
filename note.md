# Linux 基礎

## 終端機基本用法

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

## shell操作

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

## Linux系統目錄結構

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
* `/etc`:
* `/opt`:
* `/home`:
* `/tmp`: