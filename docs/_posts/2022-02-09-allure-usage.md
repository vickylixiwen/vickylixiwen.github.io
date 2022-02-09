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






