.. _waits:

异步
=========


现在很多Web应用都在使用AJAX技术。浏览器载入一个页面时，页面内的元素可能是在不同的时间载入的，
这会加大定位元素的困难程度，因为元素不在DOM里，会抛出 ``ElementNotVisibleException`` 异常，
使用 ``waits``，我们就可以解决这个问题。Waiting给(页面)动作的执行提供了一些时间间隔-通常是
元素定位或者其他对元素的操作。

Selenium WebDriver提供了两类 ``waits``- 隐式和显式。显式的 ``waits`` 会让WebDriver在更深
一步的执行前等待一个确定的条件触发，
隐式的 ``waits`` 则会让WebDriver试图定位元素的时候对DOM进行指定次数的轮询。

显式Waits
--------------

显式的 ``waits`` 等待一个确定的条件触发然后才进行更深一步的执行。
最糟糕的的做法是 ``time.sleep()``，这指定的条件是等待一个指定的时间段。
这里提供一些便利的方法让你编写的代码只等待需要的时间，``WebDriverWait`` 结合
``ExpectedCondition`` 是一种实现的方法：

::

	from selenium import webdriver
	from selenium.webdriver.common.by import By
	from selenium.webdriver.support.ui import WebDriverWait
	from selenium.webdriver.support import expected_conditions as EC

	driver = webdriver.Firefox()
	driver.get("http://somedomain/url_that_delay_loading")
	try:
		element = WebDriverWait(driver,10).until(
			EC.presence_of_element_located((By.ID,"myDynamicElement"))
		)
	finally:
		driver.quit()

这段代码会等待10秒，如果10秒内找到元素则立即返回，否则会抛出 ``TimeoutException`` 异常，
WebDriverWait默认每500毫秒调用一下 ``ExpectedCondition``直到它返回成功为止。
``ExpectedCondition`` 类型是布尔的，成功的返回值就是true,其他类型的 ``ExpectedCondition``
成功的返回值就是 ``not null``

**Expected Conditions**

自动化网页操作时，有许多频繁使用到的通用条件。下面列出的是每一个条件的实现。
Selenium + Python 提供了许多方便的方法，因此你不需要自己编写 `expected_condition` 的类，
或者创建你自己的通用包

- title_is
- title_contains
- presence_of_element_located
- visibility_of_element_located
- visibility_of
- presence_of_all_elements_located
- text_to_be_present_in_element
- text_to_be_present_in_element_value
- frame_to_be_available_and_switch_to_it
- invisibility_of_element_located
- element_to_be_clickable - it is Displayed and Enabled.
- staleness_of
- element_to_be_selected
- element_located_to_be_selected
- element_selection_state_to_be
- element_located_selection_state_to_be
- alert_is_present


::

	from selenium.webdriver.support import expected_conditions as EC

	wait = WebDriverWait(driver,10)
	element = wait.until(EC.element_to_be_clickable((By.ID,'someid')))


``expected_conditions`` 模块包含了一系列预定义的条件来和WebDriverWait使用

隐式Waits
-------------

当我们要找一个或者一些不能立即可用的元素的时候，隐式 ``waits``
会告诉WebDriver轮询DOM指定的次数，默认设置是0次。
一旦设定，WebDriver对象实例的整个生命周期的隐式调用也就设定好了。

::

	from selenium import webdriver

	driver = webdriver.Firefox()
	driver.implicitly_wait(10) # seconds
	driver.get("http://somedomain/url_that_delays_loading")
	myDynamicElement = driver.find_element_by_id('myDynamicElement')