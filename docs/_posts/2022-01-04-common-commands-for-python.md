---
layout: post
title:  "Common commands for python"
categories: python
---

#### 记录Python常用的commands

- 安装pyenv 和Python version
	`$ brew install pyenv`
	`$ pyenv install 3.6.5`
	`$ pyenv global 3.6.5`

- 安装并指定一个虚拟环境

    `$ pip install virtualenv`
    `$ mkdir ~/venv && virtualenv ~/venv/pycharm3.6.5`
    `$ source ~/venv/pycharm3.9.1/bin/activate`

-  查看pyenv多个Python version版本 [doc:](https://realpython.com/intro-to-pyenv/)
	 `$ ls ~/.pyenv/versions/`
	 `$ pyenv uninstall 3.9.0`  # 卸载某个特定python 版本
	 `$ pyenv global 3.9.1`		# 设定全局python 版本
	 `$ pyenv local 2.7.15`		# 设定局部python 版本

- 查找具体安装的lib信息

	`$ pip show Appium-Python-Client`

	>Name: Appium-Python-Client <br>
	Version: 0.28 <br>
	Summary: Python client for Appium 1.5 <br>
	Home-page: http://appium.io/ <br>
	Author: Isaac Murchie <br>
	Author-email: isaac@saucelabs.com <br>
	License: Apache 2.0 <br>
	Location: /Users/vicky/venv/pycharm3.6.5/lib/python3.6/site-packages <br>
	Requires: selenium <br>
	Required-by: <br>


- list all of the cron jobs 
    `$ crontab -l`


- edit cron jobs
    `$ crontab -e`


- 查看多个Java版本
	`$ /usr/libexec/java_home -V`


- 查看已安装ruby 版本号
	`rbenv install --list`

0 */4 * * *


* * * * * command(s)
^ ^ ^ ^ ^
| | | | |     allowed values
| | | | |     -------
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)