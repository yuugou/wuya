# PySide6 / PyQt6 框架的区别

## 代码的互相替换

PySide6 和 PyQt6 都是 Python 的 Qt 绑定库，用于开发跨平台的 GUI 应用程序。虽然它们都基于 Qt 6，基本上没有差别,通过以下方式可以互相替换:

```python
from PySide6 import * 
from PyQt6 import * 
```

## 代码的主要差异

PySide 6/PyQt 6 之间有两个最重要的区别,基本上可以帮助开发人员解决 PySide 6/PyQt 6 之间约 95%的兼容性问题。

### 信号与槽的命名不同

PySide 6 和 PyQt 6 关于信号与槽的命名不同，使用下面的方法可以统一起来：

```python
from PySide6.QtCore import Signal,Slot
from PyQt6.QtCore import  pyqtSignal as Signal,pyqtSlot as Slot 
```

### 是否可以使用枚举的快捷方式

- PySide 6 为枚举的选项提供了快捷方式

    如 Qt.DayOfWeek 枚举包括星期一到星期日的 7 个值，在 PySide 中星期三可以直接用 Qt.Wednesday 表示，而在 PyQt 6 中需要完整地使用 Qt.DayOfWeek.Wednesday 表示(PySide6 中也可以使用这种方式)。

    ???+ Note
        PySide6 快捷方式的写法,在 VSCode 中, Pylance 会提示错误, 但是运行时是没有问题的。
        ```python
        Cannot access attribute "Wednesday" for class "type[Qt]"
        Attribute "Wednesday" is unknown Pylance(reportAttributeAccessIssue)
        ```

- 但在PyQt 6 中不可以使用快捷方式。

解决这个问题最简单的方法是从 Qt 官方的帮助文档中查询枚举的完整路径。
