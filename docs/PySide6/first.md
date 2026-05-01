# 第一个 PySide6 应用

## 第一个 PySide6 应用的完整代码

在本节中，我们将创建一个简单的 PySide6 应用程序，展示如何使用 PySide6 创建窗口和添加基本组件。内容参考了于官方示例.

代码创建了一个简单的窗口应用程序，窗口包含一个标签和一个按钮。初始时标签显示"Hello,World"。每次点击按钮，标签会随机显示一种语言的问候语（从中文、法语、西班牙语中随机选择）。

```python
# -*- coding: utf-8 -*-

import sys
import random
from PySide6.QtCore import Qt, Slot
from PySide6.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton

class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))

if __name__ == "__main__":
    app = QApplication(sys.argv)

    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```

## 分步讲解

### 0. 设置代码使用 `utf-8` 编码

```python
# -*- coding: utf-8 -*-
```

### 1. 导入必要的模块

```python
import sys
import random
from PySide6.QtCore import Qt, Slot
from PySide6.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton
```

- 导入模块
    - `sys`: 提供了与 Python 解释器交互的功能,例如获取命令行参数,设置程序的退出码.
    - `random`: 用于生成随机数,在这里用来随机选择问候语.
    - `PySide6.QtCore`: 包含核心非 GUI 功能,如信号和槽机制.
    - `PySide6.QtWidgets`: 包含创建 GUI 组件的类,如窗口、布局、标签和按钮. QWidget 是大部分控件的父类.

### 2. 创建主窗口类 `MyWidget`

- 定义一个自定义窗口类 `MyWidget`，继承自 `QWidget`，作为应用程序的主窗口。

```python hl_lines="1"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 定义 `__init__` 方法是类的构造函数，用于初始化窗口。
    - `super().__init__()` 调用父类 QWidget 的构造函数

```python hl_lines="2 3"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 创建一个包含多语言问候语的列表。

```python hl_lines="5"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 布局设置：创建垂直布局管理器，并关联到当前窗口

```python hl_lines="7"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 创建标签组件，初始文本为 "Hello,World"，并设置文本居中对齐。

```python hl_lines="8"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 创建按钮组件，文本为 "Click me"。

```python hl_lines="9"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 将标签和按钮添加到垂直布局中。先将标签添加到垂直布局, 再将按钮添加到标签下方

```python hl_lines="10 11"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 信号槽连接：当按钮被点击时，调用代码下文中定义的 `magic()` 方法。

```python hl_lines="13"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot()
    def magic(self):
        self.label.setText(random.choice(self.hello))
```

- 槽函数：定义 `magic()` 方法，当按钮被点击时调用。

```python hl_lines="2 3"
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()

        self.hello = ["你好,世界","Bonjour le monde","Hola Mundo"]
        
        self.layout = QVBoxLayout(self)
        self.label = QLabel("Hello,World",alignment=Qt.AlignCenter)
        self.button = QPushButton("Click me")
        self.layout.addWidget(self.label)
        self.layout.addWidget(self.button)
        
        self.button.clicked.connect(self.magic)
    
    @Slot() # (1)
    def magic(self):
        self.label.setText(random.choice(self.hello)) # (2)
```

1. `@Slot()` - 这是一个装饰器，用于标记方法为 Qt 的槽函数，使其可以被信号调用。
2. 槽函数的作用是:随机选择问候语并更新标签文本。

!!! note
    槽函数的定义并不是在 `__init__` 函数当中，而是作为 `MyWidget` 类的一个方法定义。

### 3. 主程序入口

- 主程序入口：确保代码仅在直接运行时执行

```python hl_lines="1"
if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```

- 创建应用 QApplication 实例, 每个 PySide 6 程序都需要有一个 QApplication 对象.
- Python 脚本从 Shell 中执行时,可以通过 `sys.argv` 获取命令行参数.

```python hl_lines="2"
if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```

- 创建窗口实例：实例化自定义的 MyWidget

```python hl_lines="4"
if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```

- 设置窗口尺寸：宽度 640 像素，高度 360 像素

```python hl_lines="5"
if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```

- 显示窗口：使窗口可见

```python hl_lines="6"
if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```

- 启动事件循环：进入 Qt 主循环，确保应用程序持续运行
    - `exec()` 是 QApplication 类的一个方法,用于进入 Qt 主循环，处理事件和信号,直到 `exit()` 被调用(退出应用程序).
    - `sys.exit()` 确保程序退出时返回正确状态码, 系统的环境变量会记录状态码.如果程序运行成功，那么 exec()函数的返回值为 0，否则为非 0。

```python hl_lines="8"
if __name__ == "__main__":
    app = QApplication(sys.argv)
    
    widget = MyWidget()
    widget.resize(640,360)
    widget.show()

    sys.exit(app.exec())
```
