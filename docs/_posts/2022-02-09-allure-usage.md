---
layout: post
title:  "Glow UI automation rules"
date:   2022-02-09 
categories: allure usage
---

#### rules for adding allure annotations in our ui automation cases:
- 对每个测试类添加一个story
`@allure.story("test brand new user basic scenario")`

- 对每个测试用例添加一个feature
`@allure.feature("test brand new user register function")`

- 对每个测试用例中用到的每个步骤添加一个step
`@allure.step("Step for set name for new user")`



#### rules for adding pytest mark annotations in our ui automation cases:
- 冒烟测试的用例用smoke表示，smoke后面跟上平台
`@pytest.mark.smoke_android`
`@pytest.mark.smoke_ios`

- 回归测试的用例用regression表示，regression后面跟上平台
`@pytest.mark.regression_android`
`@pytest.mark.regression_ios`

- 特定功能的用例用功能名表示
`@pytest.mark.login`



#### rules for capturing screenshots:
目前我们有2种截图
- 一种是针对用例的截图, 在用例结束后，会有一个统一的tear down的截图，会存到screenshots/teardown, 如果有失败的用例，会有一个失败的截图存到screenshots/failed并贴到allure report
- 另一种是针对测试过的页面的截图，主要是为了记录这些页面的历史UI, 会存到screenshots/pages, 对这种类型的截图，需要对应到每个具体的页面，添加截图方法并保存


#### rules for asserssion:
在UI上尽量在完成一个action后做一个assert
对于有data变动的action，也可以通过调用sync方法在server端assert，主要server端的检查只需要在sandbox上检查




