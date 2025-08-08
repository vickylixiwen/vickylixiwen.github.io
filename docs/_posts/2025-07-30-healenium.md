---
layout: post
title:  "Notes for Healenium"
date:   2025-07-30 14:03:38 +0800
categories: healenium proxy auto detect ui failure
---

### This page used to track anything I met during using the healenium. For any details, please refer to this [doc](https://healenium.io/docs/overview): 

#### Installation: 
有2种安装方式，一种是通过docker, 还有一种是shell直接安装，不过需要安装PostgerSQL等数据库相关软件，感觉可能docker安装更方便吧，那就用docker来安装吧。

解压healenium后发现有好几个docker-compose的yaml文件，因为我们整个项目都是base在appium上的，我需要的应该就是docker-compose-appium.yaml

docker-compose, 这个方法是老板docker才使用的命令，新版的话，直接使用docker compose即可

`$ docker compose -f docker-compose-appium.yaml up` - 启动healenium server 

从启动的container来看，主要用到了4个image：

*   **selector-imitator：这个主要来修复locator**
*   **postgres-db: 主要存放各种locator用以后期的healing**
*   **hlm-proxy: 用于和appium、 selenium server通信**
*   **healenium: 各种backend的服务，具体可以参考这儿：**[healenium-backend](https://github.com/healenium/healenium-backend)



在这个yaml文件中主要需要修改的是：hlm-proxy下SELENIUM\_SERVER\_URL的配置，如果是本地起的appium server的话，只需要将配置改成：SELENIUM\_SERVER\_URL=<http://host.docker.internal:4726>即可

*NOTE!!!: 注意*，如果appium server 里带了bath path参数的,千万要根据自己appium的版本来决定到底要不要带这个参数，appium client与server间的联系只需要各自都加上/wd/hub就能解决版本的不一致，但是这个workaround对于healenium完全不适用，如果错误使用了带bath path参数的server，那就会跟我一样踩坑，会遇到这样的error：`Init Session: {"status":9,"value":{"error":"unknown command","message":"The requested resource could not be found, or a request was received using an HTTP method that is not supported by the mapped resource","stacktrace":""}}`。 所以，正确的appium server 配置是如果版本号是2.0的，不需要配/wd/hub, 如果是1.0的就一定要配上。

#### 踩到的坑

*   `column "version" of relation "report" does not exist` - 检查了healnium里明确image的版本是最新的，而且在源代码里的建表的确是有version字段的, 检查postgres数据库里report表里的确没有version字段, 不是很确定啥原因，反正手工给表加上version字段后就能正常运行了。

    @Accessors(chain = true) @Data @Entity @Table(name = "report") public class Report {

    ```java
    @Id
    @Column(name = "uid")
    private String uid;

    @Column(name = "elements", columnDefinition = "jsonb")
    @Basic(fetch = FetchType.LAZY)
    @Convert(converter = RecordWrapperConverter.class)
    private RecordWrapper recordWrapper = new RecordWrapper();

    @Column(name = "create_date")
    @CreationTimestamp
    private LocalDateTime createdDate;

    @Version
    @Column(name = "version")
    private Long version;
    ```

    }

    `healenium=# select * from report;
    uid | elements | create_date -----+----------+-------------
    (0 rows)`

#### 运行后看到的结果：

一旦有脚本可以跑起来，跑完后就会直接插入到report表，比较不确定的是看到的结果不是case failed、pass的结果，而是发现某个元素的locator failed的结果
![Healing Report例图](/assets/healenium_report.png)
![Selector例图](/assets/healenium_selector.png)



