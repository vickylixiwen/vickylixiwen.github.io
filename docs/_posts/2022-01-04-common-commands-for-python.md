---
layout: post
title:  "Common commands for python"
categories: python
---

#### 记录Python常用的commands

- 安装并指定一个虚拟环境

    `$ pip install virtualenv`

    `$ source ~/venv/pycharm3.9.1/bin/activate`

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