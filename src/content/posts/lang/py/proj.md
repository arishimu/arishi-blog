---
title: Python 项目管理
published: 2026-03-04
updated: 2026-03-04
description: 'Learn Python Projects in Y minutes'
image: ''
tags: [Python]
category: 'Python'
draft: false
---

# Python 项目管理

安装 [uv](https://uv.doczh.com/getting-started/)．

## 管理 Python

**下载** Python，并**查看**本机已安装的 Python：

```ansi
PS C:\Users\Shy_Vector\AAA> uv python install pypy
PS C:\Users\Shy_Vector\AAA> uv python list
```

```ansi
cpython-3.15.0a6-windows-x86_64-none                 [2m<download available>[0m
cpython-3.15.0a6+freethreaded-windows-x86_64-none    [2m<download available>[0m
cpython-3.14.3-windows-x86_64-none                   [2m<download available>[0m
cpython-3.14.3+freethreaded-windows-x86_64-none      [2m<download available>[0m
cpython-3.13.12-windows-x86_64-none                  [2m<download available>[0m
cpython-3.13.12+freethreaded-windows-x86_64-none     [2m<download available>[0m
cpython-3.12.13-windows-x86_64-none                  [2m<download available>[0m
cpython-3.12.10-windows-x86_64-none                  [36mC:\Program Files\Python\python.exe[0m
cpython-3.11.15-windows-x86_64-none                  [2m<download available>[0m
cpython-3.10.20-windows-x86_64-none                  [2m<download available>[0m
cpython-3.9.25-windows-x86_64-none                   [2m<download available>[0m
cpython-3.8.20-windows-x86_64-none                   [2m<download available>[0m
pypy-3.11.13-windows-x86_64-none                     [36mC:\Users\Shy_Vector\AppData\Roaming\uv\python\pypy-3.11.13-windows-x86_64-none\pypy3.11.exe[0m
pypy-3.10.16-windows-x86_64-none                     [2m<download available>[0m
pypy-3.9.19-windows-x86_64-none                      [2m<download available>[0m
pypy-3.8.16-windows-x86_64-none                      [2m<download available>[0m
graalpy-3.12.0-windows-x86_64-none                   [2m<download available>[0m
graalpy-3.11.0-windows-x86_64-none                   [2m<download available>[0m
graalpy-3.10.0-windows-x86_64-none                   [2m<download available>[0m
```

**临时运行**特定版本的 Python / 脚本：

```ansi
PS C:\Users\Shy_Vector\AAA> uv run -p 3.12 python

Python 3.12.10 (tags/v3.12.10:0cc8128, Apr  8 2025, 12:21:36) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
Ctrl click to launch VS Code Native REPL
>>> exit()

PS C:\Users\Shy_Vector\AAA> uv run -p pypy python

Python 3.11.13 (413c9b7f57f5, Jul 03 2025, 18:04:37)                                                                        
[PyPy 7.3.20 with MSC v.1941 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
Ctrl click to launch VS Code Native REPL
[1;35m>>>>[0m exit()
```

**删除** Python：

```ansi
PS C:\Users\Shy_Vector\AAA> uv python uninstall pypy
```

```ansi
Searching for Python versions matching: [36mPyPy[0m
[2mUninstalled [1mPython 3.11.13[0m[2m in 777ms[0m
 [91m-[0m [1mpypy-3.11.13-windows-x86_64-none[0m
```

## 管理 venv

**初始化**项目：

```ansi
PS C:\Users\Shy_Vector\AAA> uv init -p 3.12

Initialized project `[36mAAA[0m`

PS C:\Users\Shy_Vector\AAA> ls

    目录: C:\Users\Shy_Vector\AAA

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          2026/3/4     10:53            109 .gitignore
-a----          2026/3/4     10:53              5 .python-version
-a----          2026/3/4     10:46            167 main.py
-a----          2026/3/4     10:53            154 pyproject.toml
-a----          2026/3/4     10:53              0 README.md

```

可在 `.python-version` 中修改当前项目的 Python 版本：

```text title=".python-version"
3.12
```

`pyproject.toml` 包含当前项目的配置信息，第三方工具零散的配置文件得到统一：

```text title=".pyproject.toml"
[project]
name = "AAA"
version = "0.1.0"
description = "AAAAAA"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

```

初始化之前已有 `main.py`，

```python title="main.py"
from flask import Flask

app = Flask(__name__)

@app.route('/')
def foo():
    return 'Hello World'

if __name__ == '__main__':
    app.run(debug=True)

```

里面依赖了 `flask` 第三方库，接下来我们**安装**它：

```ansi
PS C:\Users\Shy_Vector\AAA> uv add flask
```

```ansi
Using CPython 3.12.10 interpreter at: [36mC:\Program Files\Python\python.exe[0m
Creating virtual environment at: [36m.venv[0m
[2mResolved [1m9 packages[0m[2m in 1.41s
Prepared [1m8 packages[0m[2m in 533ms
Installed [1m8 packages[0m[2m in 26ms[0m
 [92m+[0m [1mblinker[0m[2m==1.9.0[0m
 [92m+[0m [1mclick[0m[2m==8.3.1[0m
 [92m+[0m [1mcolorama[0m[2m==0.4.6[0m
 [92m+[0m [1mflask[0m[2m==3.1.3[0m
 [92m+[0m [1mitsdangerous[0m[2m==2.2.0[0m
 [92m+[0m [1mjinja2[0m[2m==3.1.6[0m
 [92m+[0m [1mmarkupsafe[0m[2m==3.0.3[0m
 [92m+[0m [1mwerkzeug[0m[2m==3.1.6[0m
```

:::tip[`--dev`]
对于 [`uv add`](https://uv.oaix.tech/reference/cli/add/) 的参数，可以使用 `--dev` 参数将工具加入开发依赖组，组内的依赖仅在开发阶段被使用，不会被打包出去．

```ansi
uv add ruff --dev
```

```txt title=".pyproject.toml" ins={11-15}
[project]
name = "AAA"
version = "0.1.0"
description = "AAAAAA"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "flask>=3.1.3",
]

[dependency-groups]
dev = [
    "ruff>=0.15.4",
]

```

当然，`ruff` 是工具而不是依赖，与工程代码无关，不应该作为依赖引入．

```ansi
uv remove ruff --dev
```
:::

:::tip[`uv tool`]
对于工具，可以使用 [`uv tool install`](https://uv.doczh.com/guides/tools/#_8) 来安装：

```ansi
PS C:\Users\Shy_Vector\AAA> uv tool install ruff
[2mResolved [1m1 package[0m[2m in 265ms
Installed [1m1 package[0m[2m in 15ms[0m
 [92m+[0m [1mruff[0m[2m==0.15.4[0m
Installed 1 executable: [1mruff[0m
PS C:\Users\Shy_Vector\AAA> which ruff
C:\Users\Shy_Vector\.local\bin\ruff.EXE
PS C:\Users\Shy_Vector\AAA> ruff check
All checks passed!
```

查看所有工具：

```ansi
PS C:\Users\Shy_Vector\AAA> uv tool list
[1mruff v0.15.4[0m
- ruff
```
:::

我们可以看到 uv 已经为当前项目**自动创建虚拟环境** `.venv`：

```ansi
PS C:\Users\Shy_Vector\AAA> tree
```

```ansi
文件夹 PATH 列表
卷序列号为 EA1E-9D15
C:.
├─.ruff_cache
│  └─0.15.4
└─.venv
    ├─Lib
    │  └─site-packages
    │      ├─blinker
    │      │  └─__pycache__
    │      ├─blinker-1.9.0.dist-info
    │      ├─click
    │      │  └─__pycache__
    │      ├─click-8.3.1.dist-info
    │      │  └─licenses
    │      ├─colorama
    │      │  ├─tests
    │      │  └─__pycache__
    │      ├─colorama-0.4.6.dist-info
    │      │  └─licenses
    │      ├─flask
    │      │  ├─json
    │      │  │  └─__pycache__
    │      │  ├─sansio
    │      │  │  └─__pycache__
    │      │  └─__pycache__
    │      ├─flask-3.1.3.dist-info
    │      │  └─licenses
    │      ├─itsdangerous
    │      │  └─__pycache__
    │      ├─itsdangerous-2.2.0.dist-info
    │      ├─jinja2
    │      │  └─__pycache__
    │      ├─jinja2-3.1.6.dist-info
    │      │  └─licenses
    │      ├─markupsafe
    │      │  └─__pycache__
    │      ├─markupsafe-3.0.3.dist-info
    │      │  └─licenses
    │      ├─werkzeug
    │      │  ├─datastructures
    │      │  │  └─__pycache__
    │      │  ├─debug
    │      │  │  ├─shared
    │      │  │  └─__pycache__
    │      │  ├─middleware
    │      │  ├─routing
    │      │  │  └─__pycache__
    │      │  ├─sansio
    │      │  │  └─__pycache__
    │      │  ├─wrappers
    │      │  │  └─__pycache__
    │      │  └─__pycache__
    │      ├─werkzeug-3.1.6.dist-info
    │      │  └─licenses
    │      └─__pycache__
    └─Scripts
```

:::note[虚拟环境何以实现？]
当虚拟环境被激活后，`.venv\\Lib\\site-packages` 目录将被追加进 Python 中 `sys.path` 列表：

```python
>>> import sys, pprint
>>> pprint.pp(sys.path)
['',
 'C:\\Program Files\\Python\\python312.zip',
 'C:\\Program Files\\Python\\DLLs',
 'C:\\Program Files\\Python\\Lib',
 'C:\\Program Files\\Python',
 'C:\\Users\\Shy_Vector\\AAA\\.venv',
 'C:\\Users\\Shy_Vector\\AAA\\.venv\\Lib\\site-packages']
```

Python 在导入模块时，将逐个检查 `sys.path` 列表里的每个路径，直到找出该模块．
:::

查看所有第三方库的**依赖关系**：

```ansi
PS C:\Users\Shy_Vector\AAA> uv tree
```

```ansi
[2mResolved [1m9 packages[0m[2m in 1ms[0m
AAA v0.1.0
└── flask v3.1.3
    ├── blinker v1.9.0
    ├── click v8.3.1
    │   └── colorama v0.4.6
    ├── itsdangerous v2.2.0
    ├── jinja2 v3.1.6
    │   └── markupsafe v3.0.3
    ├── markupsafe v3.0.3
    └── werkzeug v3.1.6
        └── markupsafe v3.0.3
```

现在**运行** `main.py`：

```ansi
PS C:\Users\Shy_Vector\AAA> uv run main.py
 * Serving Flask app 'main'
 * Debug mode: on
[91m[1mWARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.[0m
 * Running on http://127.0.0.1:5000
[93mPress CTRL+C to quit[0m
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 329-036-685
127.0.0.1 - - [04/Mar/2026 10:59:36] "GET / HTTP/1.1" 200 -
```

:::important[我拿到的是别人的项目，怎么配环境？]

难不成只能看 `pyproject.toml`，一个个手动 `uv add`？当然不需要！只需

```ansi
PS C:\Users\Shy_Vector\AAA> uv sync
```

uv 就会读取 `pyproject.toml`，自动搭建虚拟环境，并安装好所有依赖．
:::

:::tip[Python 往事]
如果只用 Python 官方的工具管理项目，那么流程是：手动创建虚拟环境 `.venv` 并激活，编辑并读取 `pyproject.toml`，往 `.venv` 安装依赖．

```bash
python -m venv .venv
source .venv/bin/activate (.venv\\Scripts\\activate)
edit pyproject.toml
pip install -e .
```

> `.venv` 可以取别的名字，但 `.venv` 这个名字已经被各界认同，也便于 IDE 识别．

> `pip install -e .` 是**自安装**：
>
> 1. `pip` 先读取 `pyproject.toml`，**把当前项目打包**成标准的 Python 软件包；
> 2. `pip` 会**像安装任何第三包那样，安装**刚刚打包好的软件包 (含依赖)．
>
> > 1. 当前项目的源代码也会被打包进去 (比如 `main.py` 也被放进了 `site-packages` 目录下)；
> > 2. 这样做的好处是：我们的源代码在导入本项目别的代码时，可以不用相对导入 (如 `from . import foo`)，而用**绝对导入** (如 `import AAA.foo`)，这样写会**与项目使用者写法保持一致**，语义更清晰；
> > 3. 但这样就存在两份一样的源代码，会引入修改同步问题．此时使用 `-e` 参数 (编辑模式)，`site-packages` 目录下的源代码将变成链接文件，指向项目源代码．
>
> `uv run` 已经自动在运行脚本之前帮我们自安装好啦，不用操心．
:::

:::tip[Python 传说]
在 Python 官方没有标准规定 `pyproject.toml` 文件之前，若想让别人复现这个环境，需使用

```bash
pip freeze > requirements.txt
```

把所有包导出到 `requirements.txt`：

```text title="requirements.txt"
flask==3.1.3
blinker==1.9.0
click==8.3.1
itsdangerous==2.2.0
jinja2==3.1.6
markupsafe==3.0.3
werkzeug==3.1.6
```

但 `requirements.txt` 混合了所有的直接或间接依赖，很难维护 (比如删除 `flask` 之后，剩余的间接依赖仍被保留，因为 `requirements.txt` 并不包含依赖关系的信息)．
:::

## 项目结构

**flat layout**：

```ansi
AAA
├─AAA
│  ├─app.py
│  ├─bar.py
│  └─foo.py
├─...
├─docs
├─...
├─tests
├─...
├─pyproject.toml
├─README.md
└─...
```

**src layout**：

```ansi
AAA
├─docs
├─...
├─src
│  └─AAA
│     ├─app.py
│     ├─bar.py
│     └─foo.py
├─tests
├─...
├─pyproject.toml
├─README.md
└─...
```

:::tip[为什么要套娃]
如果没有套一层，项目打包并解压到 `site-packages` 后，根目录的源代码会裸露在 `site-packages` 里，此时会有导入污染的问题：

```python
import app, bar, foo # 万一同时有 AAA.app 和 BBB.app 怎么办？
```

如果套了一层，就不会有这个污染问题：

```python
import AAA.app
from AAA import app, bar, foo
```
:::

## 打包

:::note[认识 `.whl`]
当我们使用 `pip`，`uv`，`poetry` 这些工具安装软件包时，都会在 [PyPI](https://pypi.org/) 下载相应的 `.whl` 文件 (如 `flask-3.1.3-py3-none-any.whl`)，其本质就是**压缩包**．

所谓的 `pip install`，就是将 `.whl` 压缩包解压到 `site-packages` 目录里

```ansi
└─.venv
    ├─Lib
    │  └─site-packages
    │      ├─__pycache__
    │      ├─...
    │      ├─AAA
    │      │  ├─__pycache__
    │      │  ├─module
    │      │  │  ├─cat.py
    │      │  │  ├─dog.py
    │      │  │  └─...
    │      │  ├─app.py
    │      │  ├─bar.py
    │      │  ├─foo.py
    │      │  └─...
    │      ├─AAA-0.1.0.dist-info
    │      │  └─...
    │      ├─...
    │      ├─flask
    │      │  ├─__pycache__
    │      │  └─...
    │      ├─flask-3.1.3.dist-info
    │      │  └─...
    │      └─...
    └─Scripts
```

> 1. `flask` 目录存放着该库的所有源代码，此时可以使用 `import flask.app` 来导入 `app.py` 文件．
> 2. `flask-3.1.3.dist-info` 目录里的 `METADATA` 文件由 `pyproject.toml` 生成，记录了所需依赖．
:::

:::note[Python 构建系统]
`.whl` 文件如何生成？

Python 的构建系统分为**前端** (命令行工具，官方推荐工具为 `build`) 和**后端** (把项目代码打包成 `.whl` 文件，默认使用 `setuptools` 作为后端)．由于 [PEP 517 规范](https://peps.pythonlang.cn/pep-0517/) 把前后端之间的交互接口定义得十分清晰，社区涌现了许多第三方实现，如 `flit`，`hatchling`，`uv`，`poetry`，`PDM` 等，它们都可以作为前端和后端，可以自由组合．

下面我们使用 [`build`](https://build.pypa.io/en/stable/) 作为前端，[`hatchling`](https://github.com/pypa/hatch/tree/master/backend) 作为后端．
:::

安装 `build`：

```bash
pip install build
```

根据 `hatchling` 的[要求](https://hatch.pypa.io/latest/config/build/#build-system)，配置 `pyproject.toml`：

```text title="pyproject.toml" ins={11-17}
[project]
name = "AAA"
version = "0.1.0"
description = "AAAAAA"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "flask>=3.1.3",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/AAA"]

```

现在可以打包了：

```ansi
PS C:\Users\Shy_Vector\AAA> python -m build
[1m* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:[0m
  - hatchling
[1m* Getting build dependencies for sdist...
* Building sdist...
* Building wheel from sdist
* Creating isolated environment: venv+pip...
* Installing packages in isolated environment:[0m
  - hatchling
[1m* Getting build dependencies for wheel...
* Building wheel...[0m
[92m[1mSuccessfully built [4mAAA-0.1.0.tar.gz[0m[92m[1m and [4mAAA-0.1.0-py3-none-any.whl[0m
```

> 也可以使用 `uv` 作为前端：`uv build`，比 `build` 快．

打包出来的 `.whl` 和 `.tar.gz` 存放在项目的 `dist` 目录里：

```ansi
PS C:\Users\Shy_Vector\AAA> unzip -l ./dist/AAA-0.1.0-py3-none-any.whl
Archive:  ./dist/AAA-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
      162  2020/02/02 00:00   AAA/main.py
      128  2020/02/02 00:00   AAA-0.1.0.dist-info/METADATA
       87  2020/02/02 00:00   AAA-0.1.0.dist-info/WHEEL
      280  2020/02/02 00:00   AAA-0.1.0.dist-info/RECORD
---------                     -------
      657                     4 files
```

:::tip[`__init__.py`]
`hatchling` 默认支持 src layout．

只需要添加空文件 `__init__.py` 让 `hatchling` 认为其所在的目录 `AAA` 可打包．

```text title="pyproject.toml" del={15-17}
[project]
name = "AAA"
version = "0.1.0"
description = "AAAAAA"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "flask>=3.1.3",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/AAA"]

```

```ansi ins={6}
PS C:\Users\Shy_Vector\AAA> python -m build
PS C:\Users\Shy_Vector\AAA> unzip -l ./dist/AAA-0.1.0-py3-none-any.whl
Archive:  ./dist/AAA-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2020/02/02 00:00   AAA/__init__.py
      162  2020/02/02 00:00   AAA/main.py
      128  2020/02/02 00:00   AAA-0.1.0.dist-info/METADATA
       87  2020/02/02 00:00   AAA-0.1.0.dist-info/WHEEL
      354  2020/02/02 00:00   AAA-0.1.0.dist-info/RECORD
---------                     -------
      731                     5 files
```
:::

可以 `pip install` 或者 `uv add` 将 `.whl` 文件解压至 `.venv\\Lib\\site-packages`，实现软件包的安装．
