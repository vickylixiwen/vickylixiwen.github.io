---
layout: post
title:  "Tips for appium Android element locator!"
categories: appium
---

### 除了xpath外，Android元素的另一种获取方式：uiSelector
对于一些元素共享一个ID，除了万能的XPATH外，还可以选择使用android的ANDROID_UIAUTOMATOR


实例：
![元素snippet](/assets/android_uiautomator_example1.png "element snippet")

{% highlight python %}
'android_uiautomator,  new UiSelector().text("28 days")'
{% endhighlight %}

这个元素就属于不太好获取的，可以尝试用ANDROID_UIAUTOMATOR来获取，上面的实例获取的是28 days这个元素

同样的UIScrollable也是常用的获取方式， 除了下面的例子，还可以参考: [appium-docs](https://appium.io/docs/en/writing-running-appium/android/uiautomator-uiselector/)

{% highlight python %}
    # this function only support for android UiScrollable element
    def scroll_to_element_text(self,   element_text,   driver):
        return driver.find_element(
            MobileBy.ANDROID_UIAUTOMATOR,  
            'new UiScrollable(new UiSelector().scrollable(true).instance(0))'
            '.getChildByText(new UiSelector().className("android.widget.TextView"),   "' + element_text + '")')
{% endhighlight %}


### 获取元素坐标,   屏幕坐标的方法：

{% highlight python %}
    element = self.get_element(ele,   driver)
    width = element.size.get("width")
    height = element.size.get("height")
    ele_x = element.location.get("x")
    ele_y = element.location.get("y")
{% endhighlight %}


{% highlight python %}
    window_size = driver.get_window_size()
    width = window_size['width']
    height = window_size['height']
{% endhighlight %}



### adb的一些方法：
{% highlight python %}
    # this is a function only for android
    # to use this function,   you need to click the edit text first
    def adb_input(self,   element_locator_str,   driver,   value):
        self.element_click_wait(element_locator_str,   driver)
        device_name = driver.desired_capabilities["deviceName"]
        adb_command = "adb -s %s shell input text %s" % (device_name,   value)
        os.system(adb_command)
{% endhighlight %}


### date picker的几种获取方式及完成所需时间：
具体环境：(不同环境，耗时可能会有偏差)
- Appium v2.0.0-beta.23
- uiautomator2@1.73.0

方法1: uiautomator获取所需年份，点击完成，总耗时1秒

方法2: xpath获取日期picker，点击前一天的日期设置生日日期，总耗时1秒

方法3: xpath获取月份picker，向下滚动到下一个月份,   总耗时2秒

方法4: sendKeys， 这个方法已经失效很久

先通过ID获取所有picker元素， 再根据元素的index具体拿到月份的picker，在把AUG这个值通过sendKey的方法传进去

	

{% highlight python %}
	"_birthday_pickers": "id,   android:id/numberpicker_input"
	picker_locator = self.get_page_element_locators('_birthday_pickers',   platform)
    pickers = self.get_elements(picker_locator,   driver)
    print(len(pickers))
    # sendKeys set month to Aug
    # sendKeys方法封装在了ui_base文件的type_in_value里，具体参考该文件
    self.type_in_value(pickers[0],   "Aug",   driver)  
{% endhighlight %}


具体实例：
![Android picker snippet](/assets/android_picker_example.png "Android picker snippet")
{% highlight python %}
	# 具体元素locator
    "_birthday_pickers": "id,   android:id/numberpicker_input",  
    "_birthday_year_picker": 'android_uiautomator,   new UiSelector().text("1984")',  
    "_birthday_date_picker": "xpath,   //android.widget.NumberPicker[2]/android.widget.Button",  
    "_birthday_month_picker": "xpath,   //android.widget.NumberPicker",  

    # uiautomator获取年的picker，然后点击选取年份
    uiautomator_t1 = int(time.time())
    print(uiautomator_t1)   # 1640749811
    year_ele_locator = self.get_page_element_locators('_birthday_year_picker',   platform)
    year_ele = self.get_element(year_ele_locator,   driver)
    uiautomator_t2 = int(time.time())
    print(uiautomator_t2)    # 1640749812
    print("ui automator selector used %s seconds" % (uiautomator_t2 - uiautomator_t1)) 
    self.element_click_wait(year_ele,   driver)
    uiautomator_t3 = int(time.time())
    print(uiautomator_t3) # 1640749812
    print("click ui automator selector used %s seconds" % (uiautomator_t3-uiautomator_t1))
    
    # xpath获取日期picker，点击前一天的日期设置生日日期
    # xpath to adjust date to previous date
    xpath_t1 = int(time.time())
    print(xpath_t1)    # 1640749812
    date_picker_locator = self.get_page_element_locators('_birthday_date_picker',   platform)
    date_picker = self.get_element(date_picker_locator,   driver)
    xpath_t2 = int(time.time())
    print(xpath_t2)    # 1640749812
    print("xpath selector used %s seconds" % (xpath_t2 - xpath_t1))
    self.element_click_wait(date_picker,   driver)
    xpath_t3 = int(time.time())
    print(xpath_t3)    # 1640749813
    print("click xpath selector used %s seconds" % (xpath_t3 - xpath_t1))

    # xpath获取月份picker，向下滚动到下一个月份
    # mobile swipe
    swipe_t1 = int(time.time())
    print(swipe_t1)    # 1640749813
    month_picker_locator = self.get_page_element_locators('_birthday_month_picker',   platform)
    month_picker = self.get_element(month_picker_locator,   driver)
    swipe_t2 = int(time.time())
    print(swipe_t2)    # 1640749814
    print("xpath selector used %s seconds" % (swipe_t2 - swipe_t1))
    driver.execute_script('mobile: scrollGesture',   {
        'elementId': month_picker.id,  
        'direction': 'down',  
        'percent': 3.0
    })
    swipe_t3 = int(time.time())
    print(swipe_t3)    # 1640749815
    print("xpath selector used %s seconds" % (swipe_t3 - swipe_t1))


{% endhighlight %}


