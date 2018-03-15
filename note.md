# Linux 基礎

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
單橫線 + 字母 [ `-r | -p | -l`]

雙橫線 + 單字 [ `--help | --version` ]