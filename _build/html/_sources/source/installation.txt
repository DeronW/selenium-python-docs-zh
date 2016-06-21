.. _installation:

安装
=======

介绍
------

Selenium Python 提供了一个简单的API 便于我们使用 Selenium WebDriver编写 功能/验收测试。
通过Selenium Python的API，你可以直观地使用所有的 Selenium WebDriver 功能

Selenium Python提供了一个很方便的接口来驱动 Selenium WebDriver ，
例如Firefox、Chrome、Ie,以及Remote，目前支持的python版本有2.7或3.2以上.

当前文档仅针对 Selenium2, 不包括 Selenium 1 和 Selenium RC.

下载 Selenium 的 Python 库
-------------------------------

推荐使用 ``pip`` 来安装Selenium 的 Python 包.
也可以从 http://pypi.python.org/pypi/selenium 下载 .
Python3.5 默认包含了 pip，用pip安装 Selenium:

::

	pip install selenium

你可能会想用虚拟机来安装一个独立的Python环境，Python的 虚拟环境_. 功能和虚拟机基本上是一样的.

Windows用户的详细说明
------------------------

.. note::

	你需要联网来完成这个安装过程

1. 安装 Python3.5 在 http://www.python.org/download 下载MSI安装包
2. 用 ``cmd.exe`` 开启命令行，并用下面的命令安装 ``selenium``

::

	C:\Python35\Scripts\pip.exe install selenium

现在你可以用python来运行你的测试脚本了.
例如，如果你创建了一个Selenium脚本然后保存为 ``C:\my_selenium_script.py`` ,然后运行

::

	C:\Python35\python.exe C:\my_selenium_script.py

下载 Selenium Server
-----------------------

.. note::

	只有在远程调用 WebDriver 时才需要用到 Selenium Server.
	通过 :ref:`selenium-remote-webdriver` 章节查看更多细节.
	如果是初学 Selenium, 可以跳过这个部分内容

Selenium server 是个Java程序，推荐使用1.6及以上的JRE来运行Selenium server。

在 http://seleniumhq.org/download/ 下载 2.x 版本的 Selenium server，
文件名看起来应该类似于这样 ``selenium-server-standalone-2.x.x.jar``
Selenium server 2.x 保持向后兼容.

如果你的机器没有安装JRE，你可以从 http://www.oracle.com/technetwork/java/javase/downloads/index.html 下载一个。
如果你使用的是  GNU/Linux 系统并且有ROOT权限的话，你也可以使用系统命令来安装JRE.
配置好 Java 的环境变量后, 可以启动 Selenium server

::

	java -jar selenium-server-standalone-2.x.x.jar

.. _虚拟环境: http://docs.python.org/3.4/using/scripts.html#scripts-pyvenv