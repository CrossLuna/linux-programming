## 00. 複習
### `vim`
* 三個模式: 命令、編輯、末行
### `gcc`
* `-I`
* `-c`
* `-o`
* `-Wall`
* `-D`
* `-On`
* `-g`
### 動態庫和靜態庫
* 是什麼?  
    二進制的源碼
* 能幹什麼?  
    加密、保護知識產權
* 怎麼用?  
    - 靜態庫
        1. 生成`.o`: `gcc a.c b.c -c`
        2. 打包: `ar rcs libtest.a a.o b.o`
        3. 使用: 編寫測試程序`test.c`  
            `gcc test.c -L./ -ltest`
    - 動態庫
        1. 生成`.o`: `gcc a.c b.c -c -fpic`
        2. 打包: `gcc -shared a.o b.o -o libtest.so`
        3. 使用: 編寫測試程序`test.c`  
            `gcc test.c -L./ -ltest`
* 優缺點?  


## 01. `makefile`
### `make`
* `gcc` - 編譯器
* `make` - Linux自帶的構建器，構建的規則在makefile中

### makefile文件的命名
* `makefile`
* `Makefile`

### makefile中的規則  
`gcc a.c b.c c.c -o app`
* 三部份: 目標、依賴、命令
```
#目標:依賴
#(tab縮進) 命令
app:a.c b.c c.c
    gcc a.c b.c c.c -o app
```
* makefile 中由一條或是多條規則組成

### 編寫makefile

#### 第一個版本  
```
app:main.c add.c sub.c mul.c
    gcc main.c add.c sub.c mul.c -o app
```
缺點:效率太低，修改一個文件，所有文件都會被全部重新編譯
    
#### 第二個版本  
* 工作原理
    + 檢測依賴是否存在：向下搜索下面的規則，如果有規則是用來生成查找的依賴，則執行規則中的命令
    + 依賴存在，判斷是否需要更新
        - 原則：目標時間 > 依賴的時間 ，反之，則需要更新
    
```
app:main.o add.o sub.o mul.o
    gcc main.o add.o sub.o mul.o -o app
main.o:main.c
    gcc main.c -c
add.o:add.c
    gcc add.c -c
sub.o:sub.c
    gcc sub.c -c
mul.o:mul.c
    gcc mul.c -c
```

缺點: 冗餘

#### 第三個版本
* 自定義變量，習慣上用小寫
    `obj=a.o b.o c.o`  
    `obj=10`
* 變量的取值 `aa=$(obj)`
* `makefile` 自帶的變量：大寫
    + `CPPFLAGS`
    + `CC`
* 自動變量
    + `$@` 規則中的目標，比如 `app`
    + `$<` 規則中的第一個依賴，比如 `main.o`
    + `$^` 規則中的所有依賴，比如 `main.o add.o sub.o mul.o`
    + 只能在規則(第二行)的命令中使用，不能在目標或依賴中使用，模式匹配時必須要用
* 模式匹配
    + `%.o:%.c`
        - 第一次: `main.o`沒有  
        ```
        main.o:main.c
            gcc -c main.c -o main.o
        ```
        - 第二次: `add.o` 沒有
        ```
        add.o:add.c
            gcc -c add.c -o add.o            
        ```
```
obj = main.o add.o sub.o mul.o
target = app
$(target):$(obj)
    gcc $(obj) -o $(target) 
    # 或是 gcc $^ -o $@

# %是一個匹配符
%.o:%.c
    gcc -c $< -o $@
```

缺點: 可移植性比較差

#### 第四個版本 - 使用函數
* `makefile`中所有的函數都有返回值
* 查找指定目錄下指定類型的文件
    + `src = $(wildcard ./*.c)`
* 匹配替換(把`.c`換成`.o`)
    + `obj = $(patsubst %.c, %.o, $(src))`
```
src = $(wildcard ./*.c)
obj = $(patsubst %.c, %.o, $(src))
target = app
$(target):$(obj)
    gcc $(obj) -o $(target) 
%.o:%.c
    gcc -c $< -o $@
```
缺點: 不能清理項目

#### 第五個版本
* 讓`make`生成不是終極目標的目標 `make 目標名`
* 編寫一個清理項目的規則，強制執行`-f`  
    ```
    clean:
    -rm $(obj) $(target) -f 
    ```
* `rm`之前加上減號表示，如果遇到錯誤，仍繼續執行剩下命令
* 聲明偽目標 `.PHONY:clean`，要不如果遇到一個叫做`clean`的檔，根據時間標籤規則，不會執行任何東西，聲明偽目標可以避免時間標籤檢查

```
src = $(wildcard ./*.c)
obj = $(patsubst %.c, %.o, $(src))
target = app
$(target):$(obj)
    gcc $(obj) -o $(target) 
%.o:%.c
    gcc -c $< -o $@
.PHONY:clean
clean:
    -rm $(obj) $(target) -f 
```

### 練習題
1. 編寫`makefile` [補圖]  
    在`plus`目錄下有兩個目錄`include`和`src`，請在`plus`目錄下編寫`makefile`，編譯`src`目錄下的源文件
2. `plus`有兩個不相關的文件 `a.c b.c`
    + 編寫`makefile`，`make`之後生成`a`和`b`

```

```

## 02. `gdb` 調試
### 編譯選項 `-g`
`gcc a.c b.c c.c -o app -g`  
加了`-g`之後，編譯器會保留函數名和變量名

### 啟動`gdb`
* `gdb [app]`
* 給程序傳入參數 `(gdb) set args xxx xxx`


### 查看代碼 `list` 或 `l`
預設顯示包含`main`的那個文件
* 當前文件
    + `l`
    + `l 行號`
    + `l 函數名`
* 非當前文件
    + `l 文件名: 行號`
    + `l 文件名: 函數名`
* 設置顯示的函數
    + `set listsize n`
    + `show listsize`

### 斷點操作 `break`或`b`
* 設置斷點
    + `b 行號`
    + `b 函數名`
    + `b 文件名: 行號`
    + `b 文件名: 函數名` 
* 查看斷點
    + `info或是i b`
* 刪除斷點
    + `d 斷點編號`
    + `d m-n` 刪除多個，從m號到n號
* 無效化斷點
    + `dis 斷點編號`
* 斷點生效
    + `ena 斷點編號`
* 設置條件斷點
    + `b 行號 if 變量==k`

### 調試相關命令
* 讓`gdb`跑起來
    + `start` 運行一行，停止
    + `run 或 r` 停在第一個斷點的位置
* 打印變量的值
    + `p 變量名`
* 打印變量的類型
    + `ptype 變量名`
* 自動持續顯示變量
    + `display 變量名`
    + `i display` 查看現有display
    + `undisplay 號碼` 取消現有的display
* 向下單步調試
    + `next 或 n` 遇到函數不會進入函數體
    + `step 或 s` 會進入到函數內部
    + `finish` 跳出函數體，如果出不去，看一下函數體中的循環是不是有斷點，如果有就刪掉
* 繼續運行`gdb`，停在下一個斷點的位置
    + `continue 或 c`
* 從循環體直接跳出
    + `until`，注意與`finish`的不同，注意不能有斷點
* 直接設置變量等於某個值
    + `set var 變量名=值`
* 退出
    + `quit 或 q`