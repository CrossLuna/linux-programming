## 複習
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


## makefile
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
