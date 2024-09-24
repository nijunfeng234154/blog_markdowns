---
title: 记录一些python的关键点
date: 2024-08-07
---

## 记录一些python的关键点

### re库与正则表达式

pattern="***（*？）**"
result = re.findall(pattern,content,re.DOTALL)
这里re.findall会返回一个列表，改好的字符调用需要使用result[0]

regex网站：

regex101.com

如何匹配网站右侧match的内容

使用re.search方法可以有效匹配第一个match的内容

在文本中全匹配例子：

```python
#匹配所有文本
function_list = []
for match in re.finditer(pattern, content):
    function_list.append(match.group())
```

## logging模块

logging模块需要初始化一个logger用来打log

```
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)
```

这样初始化了一个logger对象

或者直接使用

### loguru库

from loguru import logger

使用：

logger.info(str)  参数必须是str类型

logger.error()

logger.warning()

## OS库

如何遍历一个文件夹里的全部文件

```python
def list_files(dir):
    files_path = []
    for root, dirs, files in os.walk(dir):
        for file in files:
            if file.endswith('.c') or file.endswith('.cpp') or file.endswith('.h'):
                file_path = os.path.join(root, file)
                files_path.append(file_path)
    return files_path
```

可以把中间的判断条件改成其他类型的文件

拼接目录 使用os.path.join(a,b)

列举目录内容 使用os.listdir(dir)

获取当前目录 使用os.getcwd()

如何执行cmd命令:

例子:批量编译c语言文件

```python
def compile_main(fpath):
    exec_name = os.path.splitext(fpath)[0]

    cmd = "cd /d " + os.getcwd() + './AnghaBench-master/tools && g++ ' + fpath
    # ' -o ' + exec_name
    res = os.popen(cmd)
    output_str = res.read()
    print(output_str)
```

os.popen()方法不仅执行命令而且返回执行后的信息对象(常用于需要获取执行命令后的返回信息)，是通过一个管道文件将结果返回。通过 os.popen() 返回的是 file read 的对象，对其进行读取 read() 的操作可以看到执行的输出

#### 后来又发现有别的库可以很方便的遍历整个文件夹

```python
files = glob.glob(f"output_code3\\*.c", recursive=True)
```

很方便的匹配文件后缀

python标准库的glob

#### 在批量过程中有一些临时文件的命名

一般这些文件命名不能重复，那么我看到使用了多进程的项目中

通过获取多进程每个进程的pid来进行临时文件的命名，很好的解决了这个问题

#### python的args参数设置

一个例子来展示怎么写python args，给用户自主设置参数

```python
import argparse

parser = argparse.ArgumentParser(description='Calc')

# 添加位置参数
parser.add_argument('method', choices=['add', 'sub'], help='calc method')
parser.add_argument('-a', type=int, help='variable a')
parser.add_argument('-b', type=int, help='variable b')

# 解析参数
args = parser.parse_args()

# 使用参数
if args.method == 'add':
    print(f'{args.a} + {args.b} =', args.a+args.b)
elif args.method == 'sub':
    print(f'{args.a} + {args.b} =', args.a + args.b)
```

### 使用subprocess库时，必须要使用列表的方式来写命令行cmd命令

近期批量执行cmd时遇到报错，使用老版本os.popen不报错，但使用subprocess库报错.

最后发现这个cmd必须是一个列表格式,使用cmd.split(' ')进行构造

例子：

cmd = str()

`subprocess.run(cmd.split(' '),check=True , stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL, timeout=1000)`

### 使用multiprocessing库来多进程跑一个循环，批量处理文件

使用进程池 Pool

使用multiprocessing.Pool创建进程池，并使用pool.map(functionname,files)的形式传递函数名和文件列表，可以指定进程数量，一个进程跑一个文件。

```python
import glob
import multiprocessing

def multiple_run(file):
    # 你的处理逻辑
    print(f'Processing {file}')

if __name__ == '__main__':
    files = glob.glob('../output_yaml_raw/*.yaml')
    num_processes = 3

    try:
        # 创建一个进程池
        with multiprocessing.Pool(processes=num_processes) as pool:
            # 使用 map 将文件列表传递给 multiple_run 函数
            pool.map(multiple_run, files)
    except ValueError as e:
        print(f"Error processing files: {e}")
```
