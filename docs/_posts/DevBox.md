---
layout: post
title:  "Test on devbox"
date:   2023-01-06 
categories: devbox

---


#### Start Devbox
- install devbox, pls refer to: [Devbox Doc](https://github.com/upwlabs/devbox)
- 在DevBox repo的路径下，执行`docker compose up -d`, 开始docker上的所有service
- 回到DevBox的repo，找到`devbox/Makefile`文件，打开文件，确保你要测试的文件包含在Makefile里了。
以bryo为例，我当前要测的是bryo里的`test_discount.py`, Makefile里跑的是整个bryo的test路径下的所有UT，把bryo_ut下的UT_PATH改为`src/bryo/test/app/test_discount.py`
- `make bryo_ut`, 执行这个命令就能把刚刚配的UT跑起来

