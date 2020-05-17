---
title: "Python skill -- Pathlib"
date: 2020-05-12T12:00:00+08:00
description: "Introduce to Pathlib"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
author: Stanley
authorEmoji: 🧐
tags:
- python
- Pathlib
series:
- python skill
categories:
- python
image: images/feature3/code-file.png
---

### 目錄、路徑

#### 獲取Home目錄
```python
from pathlib import Path

Path.home()

# on Windows
output: WindowsPath('C:/Users/stanley')

# on Linux
output: PosixPath('/home/stanley')
```

<br/>

#### 獲取當前目錄
```python
from pathlib import Path

Path.cwd()

# on Windows
output: WindowsPath('C:/Users/stanley/mypathlib')

# on Linux
output: PosixPath('/home/stanley/mypathlib')
```

<br/>

#### 獲取上層目錄

使用 parent 來獲取上層目錄

```python
from pathlib import Path

Path.cwd().parent

output: PosixPath('/home/stanley')
```

<br/>

如果想要獲取再上一層的目錄

```python
Path.cwd().parent.parent

output: PosixPath('/home')
```

<br/>

如果想獲得每一層路徑，可以使用 .parents

```python
Path.cwd().parents

output: <PosixPath.parents>

list(Path.cwd().parents)

output: [PosixPath('/home/stanley'), PosixPath('/home'), PosixPath('/')]
```

<br/>

每一層資料夾的名稱

```python
Path.cwd().parts

output: ('/', 'home', 'stanley', 'mypathlib')
```

<br/>

#### 當前路徑

假設你在某個資料夾底下，想要找到目標檔案的路徑

```python
from pathlib import Path

path = Path('test.py')
path.resolve()

output: PosixPath('/home/stanley/mypathlib/test.py')
```

<br/>

#### 拼接路徑

使用 .joinpath() 方法來拼接路徑

```python
from pathlib import Path

Path('/').joinpath('home', 'stanley/mypathlib')

output: PosixPath('/home/stanley/mypathlib')
```

<br/>

更快的方法是使用 ```/``` 運算符

```python
from pathlib import Path

Path('/') / 'home' / 'stanley/mypathlib'

output: PosixPath('/home/stanley/mypathlib')
```

<br/><br/>

### 檔案、資料夾操作

#### 檔案名稱

有時候我們需要對檔案的檔名、副檔名進行操作

- **.name**：檔案全名，'test.py'
- **.stem**：檔案名稱，'test'
- **.suffix**：副檔名，'.py'

皆會以 string 方式返回

```python
from pathlib import Path

path = Path('test.py')
path.name
path.stem
path.suffix

output: 'test.py'
output: 'test'
output: '.py'
```

<br/>

如果檔案有多個副檔名

```python
from pathlib import Path

Path('test.tar.gz').suffixes

output: ['.tar', '.gz']
```

<br/>

想要更改檔名或是副檔名，可以透過 **.replace**

```python
from pathlib import Path

path = Path('test.py')

path.replace('test.txt')
```

不過要小心，path 還是原本的 ```'test.py'```

<br/>

#### 讀/寫檔案

假設現在 mypathlib 資料夾底下有 test.py 檔案

讀寫檔案的方式可以利用 with open

```python
from pathlib import Path

path = Path.cwd() / 'test.py'

with open(path, 'r') as f:
	test = [line.strip() for line in f]

print('\n'.join(test))
```

<br/>

輸出就會是

```python
# test

print('Test!!')

# end

print('End')
```

<br/>

也可以使用 **.open()** 方法來開啟

```python
with path.open('r') as f:
	...
```

除此之外，有一些比較簡易的讀寫方法

- **.read_text()**: 以 text 模式開啟，內容以 string 顯示
- **.read_bytes()**: 以 binary 模式開啟，內容以 bytestring 顯示
- **.write_text()**: 以 string 方式寫入檔案
- **.write_bytes()**: 以 binary 模式開啟並寫入檔案

<br/>

#### mkdir 與 rmdir

我們可以透過 pathlib 來建立資料夾

```python
from pathlib import Path

Path('00').mkdir()
```

<br/>

如果要刪除資料夾

```python
from pathlib import Path

Path('00').rmdir()
```

<br/>

如果要建立多層目錄的話，就需要透過 ```parents=True```
直接建立的話則會報錯

```python
from pathlib import Path

Path('00/11').mkdir(parents=True)
```

<br/>

對於刪除多層目錄，就比較麻煩了
如果在沒有淨空資料夾的情況，直接刪除的話

```python
from pathlib import Path

Path('00/11').rmdir()
```

<br/>

則會報錯

```OSError: [Errno 39] Directory not empty: '00'```

<br/>

稍微google了一下，比較好的[解決方式](https://stackoverflow.com/questions/50186904/pathlib-recursively-remove-directory)

```python
from pathlib import Path

def rm_tree(pth):
    pth = Path(pth)
    for child in pth.glob('*'):
        if child.is_file():
            child.unlink()
        else:
            rm_tree(child)
    pth.rmdir()

rm_tree('00')
```

<br/>

只要把刪除路徑的第一層資料夾放進去就可以了。

<br/><br/>

### os、pathlib 比較

最後就列出 os 與 pathlib 比較囉!

| os and os.path | pathlib |
|:---------------|:--------|
|os.path.abspath()|Path.resolve()|
|os.chmod()|Path.chmod()|
|os.mkdir()|Path.mkdir()|
|os.rename()|Path.rename()|
|os.replace()|Path.replace()|
|os.rmdir()|Path.rmdir()|
|os.remove(), os.unlink()| Path.unlink()|
|os.getcwd()|Path.cwd()|
|os.path.exists()|Path.exists()|
|os.path.expanduser()|Path.expanduser() and Path.home()|
|os.path.isdir()|Path.is_dir()|
|os.path.isfile()|Path.is_file()|
|os.path.islink()|Path.is_symlink()|
|os.stat()|Path.stat(), Path.owner(), Path.group()|
|os.path.isabs()|PurePath.is_absolute()|
|os.path.join()|PurePath.joinpath()|
|os.path.basename()|PurePath.name|
|os.path.dirname()|PurePath.parent|
|os.path.samefile()|Path.samefile()|
|os.path.splitext()|PurePath.suffix|


ref:
[1] [python-pathlib](https://docs.python.org/3/library/pathlib.html)
[2] [Python 3's pathlib Module: Taming the File System](https://realpython.com/python-pathlib/)
[3] [PathLib recursively remove directory?](https://stackoverflow.com/questions/50186904/pathlib-recursively-remove-directory)
