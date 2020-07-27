# 使用Element UI+Flask实现的浏览器主页
国内搜索引擎的信息流着实用不到，个人想关注的信息又过于分散，因此决定自己写一个主页，整合自己所需要的功能。

采用前后端分离的架构，前端使用Vue.js，后端使用Python，MariaDB+Redis提供数据存储。

前端框架使用Element UI，后端框架使用Flask，后端ORM使用Peewee。

![image](https://user-images.githubusercontent.com/27627484/86606736-fa387080-bfda-11ea-8492-30457e57295f.png)

## 部署

  后端代码位于/backend，后端入口文件位于/backend/run.py，前端代码位于/frontend，前端编译的产物位于/dist，上传的文件保存在/upload
  
  首先需要本地安装MySQL（测试时使用的是MariaDB）和Redis
  
  在具备环境后，使用python3运行根目录下的start.py并根据提示进行操作
  
  **dev分支为开发中分支，运行可能会有问题，建议使用master分支进行部署测试**
  
## 开发进度
### Done：
- 多用户登录
- 搜索引擎（支持配置多搜索引擎）
- 天气（未登录用户识别ip位置，登录用户可以自定义位置）
- 书签
- 异步脚本统一驱动和管理平台
- 接口层级的权限控制（用户、角色、权限的新增与关联）
- AppStore价格监控（支持设置触发阈值后推送微信或发送邮件）
- 黄金价格（支持设置触发阈值后推送微信或发送邮件）
- 网盘
### Working on:
- 便签
### To Do
- 翻译
- 基金
- 股票
- 新闻
  
---------------------------------------------------------------------------------------------------------------------------------

## 介绍
### 搜索
入口：主页

功能：输入内容后跳转到搜索网址，进入页面焦点自动置于搜索框内，输入文字可以带出提示（使用的百度的接口）增加引擎的话是在数据库中增加数据(search_engines)

- 自动提示和切换搜索引擎

![image](https://user-images.githubusercontent.com/27627484/71998812-3f255980-327b-11ea-9e6d-7ad97cd5c18d.png)
### 天气
入口：主页小组件

功能：如不登录则展示IP所在地的天气信息，登录后可以进行自定义，展示范围为IP+自定义位置的信息

说明：当请求数据时，为了保障速度，首先会使用缓存，缓存数据有效期为3小时（在\backend\app\weather\weather_function.py:16修改），如果没有有效缓存，则会请求外部API以获取数据

按钮：新增-登录后新增城市；排序-可拖动对自定义的城市进行排序或删除

![image](https://user-images.githubusercontent.com/27627484/87287598-df12b700-c52c-11ea-9645-60418f048f45.png)

### 书签

入口：主页小组件

功能：登陆后可以自定义，不登录时展示的书签是在数据库中修改（bookmarks.user_id==0）

按钮：新增-登陆后新增书签；设置：登陆后拖动排序、删除或修改书签的图标

- 拖动修改展示顺序

![image](https://user-images.githubusercontent.com/27627484/87288831-72002100-c52e-11ea-9fe1-aca28bfabe73.png)
- 修改书签详情

![image](https://user-images.githubusercontent.com/27627484/87288878-8512f100-c52e-11ea-8a2a-4c771ff32143.png)
- 修改书签图标

![image](https://user-images.githubusercontent.com/27627484/87288937-965bfd80-c52e-11ea-9ff2-e3d49c84d7b5.png)

### 黄金价格

入口：主页小组件

功能描述：监控黄金价格，并且可以设定阈值，当价格超过阈值时发送提醒。需要在"脚本运行平台"中配置定时任务，爬虫内部会跳过国内黄金不开盘的时间。

![image](https://user-images.githubusercontent.com/27627484/87305571-4b031880-c549-11ea-880a-115b891704db.png)

![image](https://user-images.githubusercontent.com/27627484/87305626-62da9c80-c549-11ea-8bac-f33695ac4d71.png)

### App Store应用价格监控

入口：主页小组件

功能：监控苹果软件商店应用的价格，当小于设定的阈值时，提醒用户。需要填写AppStore应用链接，此链接可以百度'想要关注的app名字+" site:apps.apple.com"'来获取，如"webssh pro site:apps.apple.com"，然后打开中文商店的页面(这样价格爬取到的才是中文)，此时的页面链接即为AppStore应用链接，如"https://apps.apple.com/cn/app/id958955657"。

![image](https://user-images.githubusercontent.com/27627484/87305689-7a198a00-c549-11ea-9925-d944bdb839e5.png)

### 网盘
入口：登陆后左上角hover用户名-网盘

功能：一个简易的网盘功能，文件上传不限制大小，下载不限制速度，但是大文件上传时需要多等一会直到loading结束，文件存储于根目录的upload文件夹，并建立名称为日期的子文件夹。

![image](https://user-images.githubusercontent.com/27627484/87327430-a2fc4800-c566-11ea-8946-d24955d6448d.png)

### 控制台
入口：登陆后左上角hover用户名-控制台

功能：提供在console表中注册的前端组件入口，方便日后权限控制或进行排序等操作，如需增加前端模块，除了编写前端业务外，还需要在console表中增加一条记录

![image](https://user-images.githubusercontent.com/27627484/87291915-a1b12800-c532-11ea-889c-d8ea54d2696b.png)

### 控制台-脚本运行平台
入口：登陆后左上角hover用户名-控制台-脚本运行平台。

功能：用于后台程序的统一驱动。前端表单均由配置产生，无需接触前端代码，通过填写配置生成的表单来提交脚本至后端运行脚本，并展示运行结果，而且具有定时运行脚本、对以前运行的任务进行回放、记录运行耗时、记录运行日志、定制运行记录表格等人性化功能

注意：
1. 为了使作为html展示的运行结果正确展示空格数量，输出的所有空格都会被替换为"\&nbsp;"，如果需要在输出中真正输出空格，则脚本输出的空格必须使用"# "来代替空格，如：自己组装html标签并增加样式的时候，标签中就必须有空格，所以需要使用"\<table# border="1"\>"代替"\<table border="1"\>"
2. 定时任务需要配置"\backend\app\script\schedule_monitor.sh"为定时运行，因为系统默认可以配置的最短的定时运行间隔为一小时，所以此定时任务的运行步长需小于一小时

模块：
- 首页：左侧为脚本所属栏目，右侧为栏目下的脚本，一个脚本为一个tab，通过右上角的"+"可以增加所选栏目的脚本
![image](https://user-images.githubusercontent.com/27627484/72076975-6dfe0700-3331-11ea-9253-717766654a2d.png)
- 按钮区域：
  右上角的五个按钮依次为：回放我上一次运行的参数（回填到表单中）、展示我上一次运行的日志、展示最近50条运行记录、配置定时运行、编辑脚本和删除按钮（此系统中所有删除和更新操作都为逻辑删除）
![image](https://user-images.githubusercontent.com/27627484/72077181-ca612680-3331-11ea-9a88-37c6ead5e6f9.png)
- 编辑脚本：
  ![image](https://user-images.githubusercontent.com/27627484/72078174-95ee6a00-3333-11ea-9d24-be5e4ff41309.png)
  
  **初始选项：**
  - 脚本名称：展示在tab上
  - 起始文件夹：配置在此处的文件夹会使用cd命令打开
  - 起始脚本：配置在此处的脚本会作为初始命令
  - 组合方式：提供两种组合方式-"顺序"和"替换"："顺序"将起始脚本和参数顺序组合提交；"替换"则类似Jenkins的处理方式，如，当配置为"python3 %参数%"的时候，系统会将"%参数%"替换为"参数"组件内填写或选择的值，当"参数"填写为"Awesome.py"的时候，最终生成的命令将是"python3 Awesome.py"
  
   **组件选项：**
  - 组件名称：前端展示的label
  - 组件类型：提供四种类型-输入框、选择器、日期选择器和日期范围选择器，根据用户配置展示为相应的组件
  - 默认值：组件默认值
  - 是否只读：前端是否只读
  - 占位文字：前端展示的placeHolder
  - 备注：备注将以组件后一个icon的方式展现，鼠标悬浮即可展示
  - 是否有额外按钮：配置为是时，组件后方会展示一个按钮，运行按钮可以运行小型脚本，适用于动态提醒用户参数，如下方图片所示。有两种数据的展示模式，一种是脚本内直接使用`print()`来打印需要输出的文字；此外，通过一定的数据格式，可以选择器组件的选项进行初始化，见下方实例：
  ```python
  import json
  d = {
      'code': 200,                                # 状态码，非必填，无实际作用
      'data': {
          'msg': 'hello\nworld qwe\nqewqweewqwe', # 展示的文本，非必须，会被解析为html，使用"\n"换行
          'value': '123',                         # 非必须，当传递value时，会将组件内的值替换成传回的值
          'options': [                            # 非必须，当传递options时，会将选择器组件内的选项替换成传回的选项
              {
                  'label': '234',                 # 标签，用于选择器组件展示的值
                  'value': '234'                  # 值，用于选择器组件选择时实际代表和传递的值
              },
          ]
      }
  }
  print(json.dumps(d))
  ```
  ![image](https://user-images.githubusercontent.com/27627484/72077217-d947d900-3331-11ea-97ce-6a7cbda6e09d.png)
  - 是否在列表展示：配置为是时，在运行列表中会以单独一列的方式呈现运行时提交的参数，但是，如果将太多的组件都设置为"在列表展示"，会带来前端性能的影响，所以建议不要设置太多个
  - 是否显示：配置为否时前端不展示
  
- 运行列表：默认展示运行人、操作、运行开始时间、耗时、运行ID五列，其中操作列包含三个按钮：参数、日志和回放。"参数"按钮悬浮可以展示参数，"日志"按钮点击可以展示日志、"回放"按钮点击可以将所选运行记录的参数回填至当前表单上。运行列表可以通过配置组件的"是否在列表展示"选项来自定义列，但不建议设置过多。
![image](https://user-images.githubusercontent.com/27627484/72077227-df3dba00-3331-11ea-9e03-b82439f5cda8.png)
- 定时任务：最小颗粒度为一小时，实际上更小也可以，但是没有试过，如果调小颗粒度，则需要减小定时任务扫库脚本的运行步长。定时任务驱动的脚本，在列表中会在运行人后方加上"(定时)"字样
![image](https://user-images.githubusercontent.com/27627484/72083322-90e1e880-333c-11ea-9995-774f0faeae73.png)

### 控制台-接口权限控制
入口：登陆后左上角hover用户名-控制台-账户和权限

功能：使用"用户-角色-权限"模型编写的接口层级的权限控制系统，接口使用一个装饰器即可以对权限进行控制。默认用户请求接口时，请求IP必须与登录时使用的IP一致，这是一种较为严格的策略，在"/backend/app/privilege/privilege_control.py:20"可以关闭

模块：
- 用户设置：新增用户、禁用用户、删除用户、修改用户角色、修改用户密码

![image](https://user-images.githubusercontent.com/27627484/87305367-f5c70700-c548-11ea-89b4-5ce69d93dd9c.png)

- 角色对应权限设置：新增角色、禁用角色、删除角色、修改角色名称、修改角色对应权限

![image](https://user-images.githubusercontent.com/27627484/87307869-eea1f800-c54c-11ea-8862-ccc5503f333a.png)

- 权限设置：新增权限、禁用权限、删除权限、修改权限

![image](https://user-images.githubusercontent.com/27627484/87305485-2444e200-c549-11ea-91cb-612b3a4c9ca5.png)

