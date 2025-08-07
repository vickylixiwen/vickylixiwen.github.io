---
layout: post
title:  "Notes for pytest upgrade!"
categories: update
---

## NOTE: 在升级前，如果你已经有一个可以work的venv，请再重新建一个venv来升级，确保正常工作不受影响
{% highlight python %}
Install virtualenv and create one to hold your project dependencies:
pip install virtualenv  # this should install to  ~/.pyenv/shims/virtualenv
mkdir ~/venv && virtualenv ~/venv/pycharm3.6.5
source ~/venv/pycharm3.6.5/bin/activate
pip install -r suso/requirements.txt  # make import thirdlibs in pycharm work
{% endhighlight %}

### 从 pytest-3.6.3 升级到 pytest-6.2.5

1. 首先遇到了allure-pytest的不兼容问题:

{% highlight python %}
pluggy._manager.PluginValidationError: unknown hook 'pytest_namespace' in plugin 
<module 'allure_pytest.plugin' from '/Users/vicky/venv/pycharm3.9.1/lib/python3.9/site-packages/allure_pytest/plugin.py'>
{% endhighlight %}

solution: 升级allure-pytest==2.9.45
$ pip install 'allure-pytest==2.9.45'

2. pytest mark warning:
{% highlight python %}
test/test_register.py:17
  /Users/vicky/Documents/Repos/automation-tests/tests/eve/ui/test/test_register.py:17: PytestUnknownMarkWarning: Unknown pytest.mark.guest_register - is this a typo?  You can register custom marks to avoid this warning - for details,   see https://docs.pytest.org/en/stable/mark.html
    @pytest.mark.guest_register

{% endhighlight %}
solution: add a pytest.ini file under ui directory

3. appium set capabilities error

`E       AttributeError: can't set attribute
/Users/vicky/venv/pycharm3.9.1/lib/python3.9/site-packages/appium/webdriver/webdriver.py:140: AttributeError`







<!-- 
Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi,   #{name}"
end
print_hi('Tom')
#=> prints 'Hi,   Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions,   you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
