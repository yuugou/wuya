# PySide6 开发工具

## Widget Development 开发工具

### Qt Designer: pyside6-designer.exe

用于设计部件用户界面的拖放工具（生成.ui文件，参见《使用Designer或QtCreator中的.ui文件配合QUiLoader和pyside6-uic》）。

### Qt User Interface Compiler: pyside6-uic.exe

用于从.ui表单文件生成Python代码。

```ps
pyside6-uic.exe -o test.py  test.ui
# 或者
# pyside6-uic -o test.py  test.ui
```

### Qt Resource Compiler: pyside6-rcc.exe

用于从.qrc资源文件编译成.py 文件,以方便 Python 直接调用（这样做的好处是 test_rc.py 文件已经包含图片资源，可以直接使用，不受原始图片位置变更的影响）

```ps
pyside6-rcc.exe -o test.py  test.qrc
# 或者
# pyside6-rcc -o test.py  test.qrc
```

## 其他工具

### Qt Assistant: pyside6-assistant.exe

pyside6-assistant.exe 是 PySide 6 的帮助文档，来源于 Qt 6 的帮助文档。可以双击运行或在命令行打开.

### Qt Linguist: pyside6-linguist.exe

pyside6-linguist.exe 为 PySide 程序增加了翻译功能，方便程序的国际化业务。通过 GUI 界面操作, 用于打开 .po 文件.

### pyside6-lupdate.exe

pyside6-lupdate.exe 用于从 Python 代码中提取所有的字符串，生成一个 .po 文件，用于后续的翻译工作。

### pyside6-lrelease.exe

pyside6-lrelease.exe 用于将 .po 文件编译成 .qm 文件，用于 PySide 程序的翻译。

### pyside6-genpyi.exe

pyside6-genpyi.exe 用于生成 .pyi 文件，用于 PySide 程序的类型提示,只能在命令行中使用.
