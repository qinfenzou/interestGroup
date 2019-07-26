# **metaBase调研**

## 一，metaBase是什么？

####  **1，应用场景：**（即，解决什么问题？）

​    针对<font color=#FF000>大量</font> 的数据报表统计图表的业务使用场景，且有时候需要对接第三方数据（<font color=#FF000>数据源的多样化，不确定性</font> ），metaBase就是便于解决此类业务场景，只需要连接需要对接的数据源，进行简单的配置，嵌入平台展示即可；

   <font color=#FF0000>补充：</font>这也是本次调研目标，明确是否能满足当前沈阳项目需求；



#### **2，支持数据库：**

- Postgres

- MySQL

- Druid

- SQL Server

- Redshift

- MongoDB

- Google BigQuery

- SQLite

- H2

- Crate

- Oracle

- Vertica

  <font color=#FF0000>补充：</font>能够支持绝大多数主流的数据库；

  

#### **3，涉及源码（二次开发）：**

   + 基础架构

     前端框架：采用<font color=#FF000>react</font>等相关框架，前端开发搭建基于yarn的开发环境，在metabase的根目录下运行：yarn run build-hot即可启动。

     后台框架：后端语言采用clojure，并结合Ring和toucan等开发框架。

     <font color=#FF0000>补充：</font>当前需求应该不需要对metaBase后端二次开发，至于前端展示只需要对相应的CSS样式进行调整；

     至少要能够修改前端样式后，前后台进行编译，能够让metabase提供正常的服务；

     

   + 开发环境推荐博客

     https://blog.csdn.net/xiaobai51509660/article/details/80585205

     

   + 编译环境搭建(必须在linux环境下进行编译)

        + **java JDK8**     jdk版本尽量不要使用其它版本【官方说>=1.6,其实非8都会有问题】

          https://www.cnblogs.com/shamo89/p/9265235.html[安装步骤]

        + **Nodejs** 

           https://nodejs.org/zh-cn/download/ [下载地址]

           https://www.cnblogs.com/zhuawang/p/7617176.html[安装步骤]

        + **Yarn**

          npm install -g  yarn

          https://www.jianshu.com/p/ca79e7ca38a4 [安装步骤]

          https://blog.csdn.net/yjaspire/article/details/89668838[环境变量配置]

        + **Leiningen**   依赖java环境

          https://leiningen.org/

          ![1563968598837](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1563968598837.png)

     <font color=#FF0000>补充：</font> 因为必须在linux环境下编译，需要了解linux基本命令：

      vi/vim, tar,yum,unzip,rz/xz,mv,rm等命令的简单使用

     

     + **编译源码**

     编译环境搭建好后;

     一、将下载的源码上传到linux系统的某个目录下，进入目录，进行解压。

     ​             cd /usr/
     ​             unzip metabase-mester.zip
     二、编译代码

             1、编译前端代码，使用命令：【yarn run build】；
         
             2、编译后端代码，使用命令：【lein ring server】；
         
             3、前后端一起编译，使用命令：【yarn run build && lein ring server】。

     三、生成metabase.jar文件

             使用命令：./bin/build，在/opt/metabase-mester/target/uberjar目录下生成一个metabase.jar文件

     ---------------------
     执行编译报了诸多问题：

        1.Tried to use insecure HTTP repository without TLS

        修改project.clj,文件开头添加：

     https://github.com/technomancy/leiningen/blob/master/doc/FAQ.md[官方问题解决集]

     ```
     ;; never do this
     (require 'cemerick.pomegranate.aether)
     (cemerick.pomegranate.aether/register-wagon-factory!
      "http" #(org.apache.maven.wagon.providers.http.HttpWagon.))
     ```

     2. 执行命令./bin/build   在工程的根目录下执行，否则会报version文件找不到；
     3. 获取不到hostname, 在/etc/hosts 中添加括号中的内容（127.0.0.1   主机名）
     4. 执行./bin/build   需要依赖gettext命令，yum install gettext 安装即可；

     <font color=#FF0000>补充：</font>之后出现的问题尽量补充在此处；

     

     前后端编译之后，执行./bin/build命令，即可在项目根目录target/uberjar/中，找到最终生成的jar:

     ![1564031924242](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564031924242.png)

     ​                                                     生成成功截图

     运行metabase.jar即可：

     java -jar metabase.jar   （启动时间有点长......） 

      [<font color =#FF000>建议:</font>在windows环境中执行该jar,linux中巨慢无比]

     ![1564037051992](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564037051992.png)

     ​                                                 windows中成功运行截图
     
     启动后，访问即可http://服务器IP:3000
     
     ![1564037291286](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564037291286.png)
     
     

***

## 二，metaBase怎么使用？

#### **1，构建metaBase服务器:**  

​    当前可用测试环境访问地址：http://172.16.239.109:3000

​     用户名:huafengming@kedacom.com  密码: root123

#### **2，发布图表：**

+ 连接数据库

  ![1563867260132](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1563867260132.png)

  ​                                                     **步骤一  进入metabase管理页面**

  ![1563867376813](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1563867376813.png)

  ​                                                  **步骤二  初始化 数据库添加入口**  

  ![1563867460439](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1563867460439.png)

  ​                                                    **步骤三   数据库连接界面**

+ 生成报表

  ![1564034562551](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564034562551.png)

  ​                                                **步骤一 选中创建图表**

  ![1564034701570](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564034701570.png)

  ​                                          **步骤二  图表生成方式**

  A.简单的单表报表生成；

  B.相对比较复杂的报表生成方式

  ![1564035310425](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564035310425.png)

  A. 选择展示的表对象；

  B.过滤条件；

  C.图表展示方式；  [足够满足报表各种展示形式]

  

+ 发布报表

​     ![1564035743762](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564035743762.png)

​                                                    **步骤一  已保存报表集**

![1564035863611](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564035863611.png)

​                                                  **步骤二  报表列表**

​         报表：即单张报表；

​         仪表盘：多张报表进行排版的集合；

   ![1564036230801](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564036230801.png)

​                                                    **步骤三  点击分享**

![1564036291867](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564036291867.png)

​                                            **步骤四  分享嵌入方式**

#### **3，平台嵌入方式：**

* 单张统计报表嵌入：

* 多张统计报表引入：

  以上两者嵌入方式一致，将公开嵌入生产的代码嵌入页面即可；

  ![1564046020745](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564046020745.png)

  ​                                                                         **图一  代码嵌入平台**

  ![1564046069212](C:\Users\wangqiang\AppData\Roaming\Typora\typora-user-images\1564046069212.png)

  ​                                                                      **图二  图表嵌入平台**

***



## 三，metaBase平台使用注意事项：

+ 样式修改

      常规修改文件路径：https://blog.csdn.net/keylkeaf/article/details/84891504 


<font color=#FF0000 >注意事项：</font>当前仅支持对CSS样式的修改；

当前gitlab上的版本已经将metabase的商标隐藏；

gitlab地址：http://gitlab.ctsp.kedacom.com/kscs/kstp-data-cube.git

官方源代码访问地址：https://github.com/metabase/metabase



