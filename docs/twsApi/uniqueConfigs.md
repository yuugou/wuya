# TWS API DocumentationTWS API 文档

## Unique Configurations / 独特的配置

While all of the available Trader Workstation API default samples provide equivalent functionality, some languages have unique configurations that must be implemented in order to use our samples or program code with the underlying API.

虽然所有可用的 Trader Workstation API 默认示例都提供相同的功能，但某些语言具有独特的配置，必须实现这些配置才能使用我们的示例或程序代码与底层 API 进行交互。

### Implementing the Intel Decimal Library for MacOS and Linux / 在 MacOS 和 Linux 上实现 Intel Decimal 库

### Updating The Python Interpreter / 更新 Python 解释器

Python has a unique system for importing libraries into it’s IDEs. This extends even further when it comes to virtual environments. In order to utilize Python code with the TWS API, you must run our setup file in order to import the code.

Python 拥有独特的系统，用于将其库导入 IDE 中。当涉及虚拟环境时，这一系统会更加复杂。为了使用 TWS API 的 Python 代码，您必须运行我们的设置文件来导入代码。

#### Open Command Prompt or Terminal / 打开命令提示符或终端

In order to update the Python IDE, these steps MUST be performed through Command Prompt or Terminal. This can not be done through an explorer interface.

为了更新 Python IDE，这些步骤必须通过命令提示符或终端执行。不能通过资源管理器界面完成。

As such, users should begin by launching their respective command line interface.

因此，用户应首先启动相应的命令行界面。

These samples will display Windows commands, though the procedure is identical on Windows, MacOS, and Linux.

这些示例将显示 Windows 命令，尽管在 Windows、MacOS 和 Linux 上的操作步骤是相同的。

![Standard command prompt window.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/setupCmd-1100x468.png)

#### Navigate to Python Source / 导航至 Python 源代码

Customers should then change their directory to ``{TWS API}\source\pythonclient``.

客户应随后将目录更改为``{TWS API}\source\pythonclient``。

It is then recommend to display the contents of the directory with “ls” for Unix, or “dir” for Windows users.

然后建议使用“ls”（适用于 Unix）或“dir”（适用于 Windows 用户）来显示目录内容。

![Contents of python source directory.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/setupCmdCd.png)

#### Run The setup.py File / 运行 setup.py 文件

Customers will now need to run the setup.py steps with the installation parameter. This can be done with the command: ``python setup.py install``

客户现在需要使用安装参数运行 setup.py 步骤。这可以通过以下命令完成：``python setup.py install``

![setup.py install command.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/setupCmdInstall.png)

#### Confirm Updates / 确认更新

After running the prior command, users should see a large block of text describing various values being updated and added to their system. It is important to confirm that the version installed on your system mirrors the build version displayed. This example represents 10.25; however, you may have a different version.

运行上述命令后，用户应看到一大段文本，描述着各种正在更新和添加到系统中的值。重要的是要确认您系统上安装的版本与显示的构建版本一致。此示例表示 10.25；但是，您可能有不同的版本。

![Updated packages from setup.py.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/setupCmdUpdate-1100x544.png)

#### Confirm your installation

Finally, users should look to confirm their installation. The simplest way to do this is to confirm their version with pip. Typing this command should show the latest installed version on your system: ``python -m pip show ibapi``

最后，用户应检查确认安装。最简单的方法是使用 pip 确认版本。输入此命令应显示系统上安装的最新版本：``python -m pip show ibapi``

![Result of pip command.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/setupCmdShowIbapi-1100x236.png)

#### Protobuf UserWarning messages / Protobuf UserWarning 消息

After resolving the reference errors, using the TWSAPI may print a UserWarning upon connection. These warnings are predominantly cosmetic and can be ignored. These issues are caused by the Pypi release of protobuf running version 6.30.1 and above, while the TWS API is built with 5.29.3. The warning is simply notifying users that their version is 1 major version different. However, given protobuf is currently backwards compatible, this should not present any issues with the implementation. Developers uncomfortable with the warning messages have a few options:

在解决引用错误后，使用 TWSAPI 在连接时可能会打印出 UserWarning。这些警告主要是美观性质的，可以忽略。这些问题是由 Pypi 发布的 protobuf 6.30.1 及以上版本引起的，而 TWS API 是用 5.29.3 构建的。警告只是通知用户他们的版本与主要版本相差 1 个版本。然而，由于 protobuf 目前是向后兼容的，这不应在实现中引起任何问题。对警告消息感到不适的开发者有几个选择：

1. [Recompile Protobuf](https://protobuf.dev/getting-started/pythontutorial/) against their [Github 5.29.3 version](https://github.com/protocolbuffers/protobuf/tree/v5.29.3) to maintain parity with the TWS API implementations.
2. [重新编译](https://protobuf.dev/getting-started/pythontutorial/)与 [Github 5.29.3 版本](https://github.com/protocolbuffers/protobuf/tree/v5.29.3)兼容的 Protobuf，以与 TWS API 实现保持一致。
3. Users can also modify the code source, linked by the protobuf warning, and simply remove lines 94 and on from the runtime_version.py file.
4. 用户也可以修改 protobuf 警告链接的代码源，并简单地从 runtime_version.py 文件中删除从第 94 行开始的行。

### Implementing Visual Basic .NET / 实现 Visual Basic .NET
