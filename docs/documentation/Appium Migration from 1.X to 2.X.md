---
layout: page
title: Appium Migration from 1.X to 2.X
permalink: /appium/
---

### This page used to track all the situations I met during the migration of Appium, for any details, please refer to this [doc](https://github.com/appium/appium/blob/2.0/docs/en/advanced-concepts/migrating-to-appium-2.0.md): 

#### Current version of appium: 
> $ appium --version

> 1.22.0

#### 开始安装:
1. 安装当前最新版本 2.0.0-beta.23
> $ npm install -g appium@2.0.0-beta.23
2. 安装结束后， 检查版本号是否升级到最新已经安装路径
> $ appium --version (2.0.0-beta.23)  
> $ which appium(/Users/vicky/.nvm/versions/node/v14.16.1/bin/appium)
3. 安装需要的各种driver， appium2.X和appium1.X的最大区别就在此：appium1.x默认安装所有driver，appium2.x可以选择需要的driver安装，具体说明请参考[appium driver cli](https://github.com/appium/appium/blob/2.0/docs/en/drivers/driver-cli.md)
> $ appium driver list (查看所有驱动)

##### Attempting to find and install driver 'xcuitest'
		✔ Installing 'xcuitest' using NPM install spec 'appium-xcuitest-driver'
		Driver xcuitest@3.59.0 successfully installed
		- automationName: XCUITest
		- platformNames: ["iOS","tvOS"]

> $ appium driver install xcuitest (安装xcuitest驱动)

> $ appium driver install uiautomator2 (安装uiautomator2驱动，主要用于Android平台)

> $ appium driver install espresso (安装espresso驱动，Android平台下的另一个driver)
我们目前用的差不多就这几个driver了，如果还需要其他driver的，可以继续安装，目前为止升级安装没有遇到任何问题


### 运行appium2.X的过程中具体遇到的问题
1. ERROR: Cannot install -r requirements.txt (line 2) and selenium==3.13.0 because these package versions have conflicting dependencies.
	root cause: appium2.0支持的是selenium 4.X, 之前我们跑的都是selenium3.X的东西，在appium升级后，selenium也需要升级
	solution: 
	> $ npm install -g selenium-webdriver@4.1.0

2. E       selenium.common.exceptions.WebDriverException: Message: The requested resource could not be found, or a request was received using an HTTP method that is not supported by the mapped resource

`/Users/vicky/venv/pycharm3.9.1/lib/python3.9/site-packages/appium/webdriver/webdriver.py:97: in __init__
    super(WebDriver, self).__init__(command_executor, desired_capabilities, browser_profile, proxy, keep_alive)
/Users/vicky/venv/pycharm3.9.1/lib/python3.9/site-packages/selenium/webdriver/remote/webdriver.py:268: in __init__
    self.start_session(capabilities, browser_profile)
/Users/vicky/venv/pycharm3.9.1/lib/python3.9/site-packages/appium/webdriver/webdriver.py:136: in start_session
    response = self.execute(RemoteCommand.NEW_SESSION, parameters)
/Users/vicky/venv/pycharm3.9.1/lib/python3.9/site-packages/selenium/webdriver/remote/webdriver.py:424: in execute
    self.error_handler.check_response(response)`

    root cause: appium2.X的wd/hub已经被移除
    solution: appium2.X server的启动命令中，添加--base-path

    > $appium  --base-path /wd/hub

3. E      pluggy._manager.PluginValidationError: unknown hook 'pytest_namespace' in plugin 
	root cause: 升级了pytest到6.2.5 (platform darwin -- Python 3.9.1, pytest-6.2.5, py-1.11.0, pluggy-1.0.0
plugins: allure-pytest-2.5.0)
	solution: 降级到python3.6.5后，(platform darwin -- Python 3.6.5, pytest-3.6.3, py-1.8.0, pluggy-0.6.0
	plugins: cov-2.8.1, celery-4.2.1, allure-pytest-2.5.0)就没有这个问题了，会有一个post专门写



4. cap需要改成dict:
caps = dict(
            platformName='iOS',
            platformVersion=iosversion,
            deviceName=iosdevice,
            automationName='XCUITest',
            app=ipapath
            # # 'noReset': 'true',
            # 'noReset': 'false',
            # 'keepKeyChains': 'true',
            # # 'bundleId':'com.upwlabs.glowdev',
            # # 'updatedWDABundleId':'com.test.webdriveragent',
            # 'newCommandTimeout': 15)









###### appium driver的其他命令
> $ appium driver install xcuitest@2.1.2 (安装某个特定版本的xcuitest驱动)

> $ appium driver list --installed(显示所有已安装的驱动)

> $ appium driver uninstall <driverName> (卸载某个驱动)

> $ appium driver install --source=<sourceType> 安装指定source的驱动， 更多信息请参考: [Installing Appium 2.0 and the Driver and Plugins CLI](https://appiumpro.com/editions/122-installing-appium-20-and-the-driver-and-plugins-cli) 




##### misc error:
python3.9的venv
`(pycharm3.9.1) vicky@Vickys-MBP ui % pytest --Platform iOS -m smoke5 -s
zsh: /usr/local/bin/pytest: bad interpreter: /usr/local/opt/python/bin/python3.7: no such file or directory
============================================================================================================================= test session starts ==============================================================================================================================
platform darwin -- Python 3.9.1, pytest-3.6.3, py-1.11.0, pluggy-0.6.0
rootdir: /Users/vicky/Documents/Repos/automation-tests/tests/eve/ui, inifile:
plugins: allure-pytest-2.5.0`


##### webdriver agent check command:
	$ cd <to your webdriver agent path> (/Users/vicky/.appium/appium-xcuitest-driver/node_modules/appium-webdriveragent)
	$ xcodebuild -project WebDriverAgent.xcodeproj \
           -scheme WebDriverAgentRunner \
           -destination 'platform=iOS Simulator,name=iPhone 13 Pro Max' \
           test
    没有任何error或者看到模拟器上已经安装WDA，就表示WDA安装成功

    


