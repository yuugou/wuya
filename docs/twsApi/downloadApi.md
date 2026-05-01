# TWS API DocumentationTWS API 文档

## Download the TWS API / 下载 TWS API

It is recommended for API users to use same TWS API version to make sure the TWS version and TWS API version are synced in order to prevent version conflict issue.

建议 API 用户使用相同的 TWS API 版本，以确保 TWS 版本和 TWS API 版本同步，以防止版本冲突问题。

Running the Windows version of the API installer creates a directory “C:\\TWS API\” for the API source code in addition to automatically copying two files into the Windows directory for the DDE and C++ APIs. **It is important that the API installs to the C: drive**, as otherwise API applications may not be able to find the associated files. The Windows installer also copies compiled dynamic linked libraries (DLL) of the ActiveX control TWSLib.dll, C# API CSharpAPI.dll, and C++ API TwsSocketClient.dll. Starting in API version **973.07**, running the API installer is designed to install an ActiveX control TWSLib.dll, and TwsRtdServer control TwsRTDServer.dll which are compatible with both 32 and 64 bit applications.

运行 Windows 版本的 API 安装程序会在"C:\\TWS API\\"目录中创建 API 源代码目录，并自动将两个文件复制到 Windows 目录中，用于 DDE 和 C++ API。**API 必须安装在 C 盘**，否则 API 应用程序可能无法找到相关文件。Windows 安装程序还会复制 ActiveX 控件 TWSLib.dll、C# API CSharpAPI.dll 和 C++ API TwsSocketClient.dll 的编译动态链接库(DLL)。从 API 版本 **973.07** 开始，运行 API 安装程序旨在安装与 32 位和 64 位应用程序兼容的 ActiveX 控件 TWSLib.dll，以及 TwsRtdServer 控件 TwsRTDServer.dll。

It is important to know that the TWS API is only available through the interactivebrokers.github.io MSI or ZIP file. Any other resource, including pip, NuGet, or any other online repository is not hosted, endorsed, supported, or connected to Interactive Brokers. As such, updates to the installation should always be downloaded from the github directly.

重要的是要知道 TWS API 只能通过 interactivebrokers.github.io 的 MSI 或 ZIP 文件获取。任何其他资源，包括 pip、NuGet 或任何其他在线仓库，都没有被托管、认可、支持或与 Interactive Brokers 连接。因此，安装更新应该始终直接从 github 下载。

[TWS API Download Page](https://interactivebrokers.github.io/)

[TWS API 下载页面](https://interactivebrokers.github.io/)

### Install the TWS API on Windows / 在 Windows 上安装 TWS API

Windows:

1. Download the IB API for Windows to your local machine
2. 将 IB API 下载到您的本地计算机
3. This will direct you to Interactive Brokers API License Agreement, please review it
4. 这将引导您查看 Interactive Brokers API 许可协议，请仔细阅读
5. Once you have clicked “I Agree“, refer to the Windows section to download the API Software version of your preference
6. 点击“我同意”后，请参考 Windows 部分下载您偏好的 API 软件版本
7. ![Highlights the TWS API versions for Windows.](https://www.interactivebrokers.com/campus/wp-content/uploads/sites/2/2023/06/twsapi-download-screenshot.jpg)
8. This will download TWS API folder to your computer
9. 这将下载 TWS API 文件夹到您的电脑
10. Go to your IDE and Open Terminal
11. 前往您的 IDE 并打开终端
12. Navigate to the directory where the installer has been downloaded (normally it should be your C: drive or D: drive) and confirm the file is present. Now, let’s take an example: install TWS API Python
13. 导航到安装程序下载的目录（通常应该是您的 C:驱动器或 D:驱动器），并确认文件存在。现在，我们以安装 TWS API Python 为例：

```powershell
cd ~/TWS API/source/pythonclient
python3 setup.py install
```

### Install the TWS API on MacOs / Linux

### TWS API File Location & Tools / TWS API 文件位置及工具

TWS API Folder Files Explanation:

TWS API 文件夹文件说明：

- “API_VersionNum.txt”

File Path: ~\TWS API\API_VersionNum.txt

文件路径: ~\TWS API\API_VersionNum.txt

You can check your API version in this file.

您可以在该文件中检查您的 API 版本。

- “IBSampleApp.exe”

File Path: ~\TWS API\samples\CSharp\IBSampleApp\bin\Release\IBSampleApp.exe

文件路径: ~\TWS API\samples\CSharp\IBSampleApp\bin\Release\IBSampleApp.exe

You can manually use the IBSampleApp to test the API functions.

您可以手动使用 IBSampleApp 来测试 API 功能。

- “ApiDemo.jar”

File Path: ~\TWS API\samples\Java\ApiDemo.jar

文件路径: ~\TWS API\samples\Java\ApiDemo.jar

This is built with Java. Java users can use it to quickly test the IB TWS API functions.

这是用 Java 构建的。Java 用户可以使用它来快速测试 IB TWS API 功能。
