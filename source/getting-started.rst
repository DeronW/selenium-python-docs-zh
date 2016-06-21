.. _getting-started:

开始
======


简单用法
--------------

如果你已经安装好Python 和 Selenium，可以这样开始使用

::

	from selenium import webdriver
	from selenium.webdriver.common.keys import Keys

	driver = webdriver.Firefox()
	driver.get("http://www.python.org")
	assert "Python" in driver.title
	elem = driver.find_element_by_name("q")
	elem.send_keys("pycon")
	elem.send_keys(Keys.RETURN)
	assert "No results found." not in driver.page_source
	driver.close()

把上面的脚本保存到一个文件中(比如：python_org_search.py)，然后就可以这样运行它了

::

	python python_org_search.py

运行时需要已经安装过 selenium 库了

用法解析
------------

`selenium.webdriver` 模块提供了所有 WebDriver的实现，现在支持的WebDriver的实现有 Firefox,Ie,Chrome 和远程浏览器.
`Keys` 类提供了键盘的按键编码（比如回车,ALT,F1等等）

::

	from selenium import webdriver
	from selenium.webdriver.common.keys import Keys

然后我们创建一个Firefox的实例

::

	driver = webdriver.Firefox()

`driver.get` 方法会打开给定的URL的页面，WebDriver会等待页面完全加载完(就是 "onload" 函数触发了),
再开始执行你的测试脚本.

但是如果你的页面用了太多的AJAX，那么这个机制就没什么卵用了，因为它不知道页面到底是什么时候加载完。

::

	driver.get("http://www.python.org")

下面代码是断言网页标题是否包含 "Python" 关键字

::

	assert "Python" in driver.title

WebDrive提供了一系统类似于 `find_element_by_*` 的方法来寻找页面元素，
例如，我们利用 `find_element_by_name` 方法，通过元素的 `name` 属性来定位一个文本输入框元素.

更多查找元素的文档参考 :ref:`locating-elements`

::

	elem = driver.find_element_by_name("q")

接着我们输入了一些字符，类似于用键盘直接输入. 特殊的键盘符我们可以用 `Key` 输入,
需要导入 `selenium.webdriver.common.keys` 包.
安全起见, 输入字符前清楚掉原来的内容, 以免产生错误结果.

::

	elem.clear()
	elem.send_keys("pycon")
	elem.send_keys(Keys.RETURN)

提交页面之后我们应该确认一下是否有返回，为了确定有返回值，我们在这里下一个断言:

::

	assert "No results found." not in driver.page_source

最后浏览器窗口被关闭了，你也可以调用 `quit` 而不是 `close` ，区别在于 `quit` 会退出整个浏览器，
而 `close` 只会关闭一个标签，但是如果浏览器只有一个标签，大部分浏览器的行为是相同的，就是关闭整个浏览器。

::

	driver.close()

使用Selenium编写测试代码
-------------------------

Selenium经常被用来写测试用例，它本身的包不提供测试的工具或者框架。
我们可以用Python的单元测试模块来编写测试用例. 或者使用 `py.test` , `nose` .

在本章节我们使用 `unittest` 做为测试框架，下面是一个用 `unittest` 模块改进后的例子，
是对 `python.org` 函数搜索功能的测试:

::

	import unittest
	from selenium import webdriver
	from selenium.webdriver.common.keys import Keys

	class PythonOrgSearch(unittest.TestCase):

		def setUp(self):
			self.driver = webdriver.Firefox()

		def test_search_in_python_org(self):
			driver = self.driver
			driver.get("http://www.python.org")
			self.assertIn("Python", driver.title)
			elem = driver.find_element_by_name("q")
			elem.send_keys("pycon")
			elem.send_keys(Keys.RETURN)
			assert "No results found." not in driver.page_source


		def tearDown(self):
			self.driver.close()

	if __name__ == "__main__":
		unittest.main()

你可以在shell里运行这个测试用例

::

	python test_python_org_search.py
	.
	----------------------------------------------------------------------
	Ran 1 test in 15.566s

	OK

上面的结果表明我们的测试用例成功执行了


测试代码分析
--------------

脚本的开头我们引入了所有需要的模块, `unittest <http://docs.python.org/library/unittest.html>`_ 模块是
Python内置的单元测试模块, 就好像 Java里面的JUnit. unittest 模块实现了WebDriver的全部测试接口的功能.
目前支持的 WebDriver 包括 Firefox, Chrome, Ie 和 远程调试. `Keys` 类提供了键盘模拟输入.

::

	import unittest
	from selenium import webdriver
	from selenium.webdriver.common.keys import Keys

测试用例类继承了 `unittest.TestCase` 类，`TestCase` 类的子类都被认为是测试用例

::

	class PythonOrgSearch(unittest.TestCase):
		pass

测试用例初始化时会首先调用 `setUp` 方法.

下面的例子中我们创建了一个 ``Firefox WebDriver`` 实例

::

	def setUp(self):
		self.driver = webdriver.Firefox()

下面的代码是一个测试某个功能点错的方法, 所有测试方法的名称都必须以 `test` 字符串开头.
方法的第一行新建了一个变量 `driver` 引用了 `setUp` 方法中创建的driver对象

::

	def test_search_in_python_org(self):
		driver = self.driver


`driver.get` 方法会打开给定的URL的页面

::

	driver.get("http://www.python.org")

下一行是个断言，确认页面标题里是否有 Python 这个单词

::

	self.assertIn("Python", driver.title)

查找元素

::

	elem = driver.find_element_by_name("q")

输入文字

::

	elem.send_keys("pycon")
	elem.send_keys(Keys.RETURN)

确认是否打开了新页面

::

	assert "No results found." not in driver.page_source

测试类的所有测试方法执行结束后, 会调用 `tearDown` 方法.
这个方法适合用来清理之前测试方法留下的残留数据.

::

	def tearDown(self):
		self.driver.close()

最后一行代码是Python的固定用法, 程序由这里开始执行

::

	if __name__ == "__main__":
		unittest.main()


.. _selenium-remote-webdriver:

使用Selenium 的 remote WebDriver
-------------------------------------

要使用remote WebDriver，你先要运行Selenium server,用下面这个命令:

::

	java -jar selenium-server-standalone-2.x.x.jar

运行Selenium server时，你可以看到类似这样一条信息:

::

	15:43:07.541 INFO - RemoteWebDriver instances should connect to: http://127.0.0.1:4444/wd/hub

意思是说你可以用这个URL连接到 remote WebDriver,下面是一些例子

::

	from selenium import webdriver
	from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

	driver = webdriver.Remote(
	   command_executor='http://127.0.0.1:4444/wd/hub',
	   desired_capabilities=DesiredCapabilities.CHROME)

	driver = webdriver.Remote(
	   command_executor='http://127.0.0.1:4444/wd/hub',
	   desired_capabilities=DesiredCapabilities.OPERA)

	driver = webdriver.Remote(
	   command_executor='http://127.0.0.1:4444/wd/hub',
	   desired_capabilities=DesiredCapabilities.HTMLUNITWITHJS)

desired_capabilities是一个字典参数，如果你不使用默认的参数，你可以自己指定

::

	driver = webdriver.Remote(
	   command_executor='http://127.0.0.1:4444/wd/hub',
	   desired_capabilities={'browserName': 'htmlunit', 'version': '2',	'javascriptEnabled': True}
	)

