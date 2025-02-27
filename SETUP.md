# 部署方法
- [部署方法](#部署方法)
  - [直接运行](#直接运行)
  - [使用 python 运行(screen)](#使用-python-运行screen)
    - [命令窗口如下](#命令窗口如下)
  - [使用青龙面板运行（V2.12+）](#使用青龙面板运行v212)
    - [1.拉取仓库](#1拉取仓库)
    - [2.环境变量添加](#2环境变量添加)
    - [3.复制配置文件](#3复制配置文件)
    - [4.添加依赖](#4添加依赖)
    - [5.编辑配置文件](#5编辑配置文件)
  - [使用 Docker 运行](#使用-docker-运行)
  - [使用云函数运行](#使用云函数运行)
  - [更多方法，请融会贯通（](#更多方法请融会贯通)

## 直接运行

1. 使用[Git](https://git-scm.com/)或[点击此处](https://github.com/ShanshanHY/MihoyoBBSTools-S/archive/refs/heads/master.zip)下载本项目

2. 下载[Python3](https://www.python.org/downloads/)

3. 解压本项目压缩包,在解压目录中**Shift+右键** 打开你的命令提示符 cmd 或 powershell

4. [requirements.txt](https://raw.githubusercontent.com/ShanshanHY/MihoyoBBSTools-S/master/requirements.txt) 是所需第三方模块，执行 `pip install -r requirements.txt` 安装模块

5. 打开目录中的**config 文件夹**复制`config.yaml.example`并改名为`config.yaml`，脚本的多用户功能靠读取不同的配置文件实现，你可以创建无数个`自定义名字.yaml`，脚本会扫描**config**目录下`yaml`为拓展名的文件，并按照名称顺序依次执行。

6. 请使用 vscode/notepad++等文本编辑器打开上一步复制好的配置文件

7. **使用[获取 Cookie](https://github.com/ShanshanHY/MihoyoBBSTools-S/blob/master/README.md#获取米游社-Cookie)里面的方法来获取米游社 Cookie**

8. 将复制的 Cookie 粘贴到`config.yaml`的`cookie:" "`中(在`account`里面)

    例子

    > ```yaml
    > cookie: 你复制的cookie
    > ```

9. 检查`config.yaml`的`enable:`的值为 true

10. 在命令提示符(cmd)/powershell，输入`python main.py`来进行执行

11. 多用户的请使用`python main_multi.py`，多用户在需要自动执行的情况下请使用`python main_multi.py autorun`

## 使用 python 运行(screen)

1. 将本项目 Clone 至本地后，安装好依赖直接运行`python3 server.py`

2. 在后台运行时请安装 screen

3. 使用`screen -S automhy`进入后台线程

4. Ctrl+A 组合键再按下 d 键回到主线程

5. `screen -r automhy`回到线程

6. 如果不能回到线程请先`screen -d automhy`挂起线程

### 命令窗口如下

    > stop: 关闭程序  
    > mulit: 测试多用户签到  
    > single: 测试单用户签到  
    > reload: 重载配置文件  
    > mod x: mod 1 为单用户模式 mod 2 为多用户模式  
    > add 'yourcookie': 直接 add cookie 添加 Cookie，根据提示输入用户存档名称  
    > time x: 设置任务巡查时间,默认 720 分钟(12 小时)  
    > set user enable true(设置 user.json 的 enable 属性为 true)  
    > show true/false: 开启/关闭 20 秒的倒计时提示

## 使用青龙面板运行（V2.12+）

### 1.拉取仓库

方式 1：订阅管理

```
名称：米游社签到
类型：公开仓库
链接：https://github.com/ShanshanHY/MihoyoBBSTools-S.git
定时类型：crontab
定时规则：2 2 28 * *
白名单：ql_main.py
依赖文件：error|mihoyo|genshin|honkai3rd|log|push|req|set|tools|con|acc|honkai2|tearsofthemis|captcha|birthdaystar|main
```

方式 2：指令拉取

```sh
ql repo https://github.com/ShanshanHY/MihoyoBBSTools-S.git "ql_main.py" "" "error|mihoyo|genshin|honkai3rd|log|push|req|set|tools|con|acc|honkai2|tearsofthemis|captcha|birthdaystar|main"
```

### 2.环境变量添加

在青龙面板环境变量中添加以下变量

| 名称                       | 值               | 功能                     |
| -------------------------- | ---------------- | ------------------------ |
| AutoMihoyoBBS_config_path  | /ql/data/config/ | 设置配置文件路径（必选） |
| AutoMihoyoBBS_config_multi | 1                | 开启多用户（可选）       |

**注意！仅多用户需添加变量```AutoMihoyoBBS_config_multi```**

### 3.复制配置文件

**进入容器后运行以下命令**（docker exec -it ql bash）修改 ql 为你的青龙容器名字

单用户请使用以下命令复制配置文件

```sh
cp /ql/data/repo/ShanshanHY/MihoyoBBSTools-S/config/config.yaml.example /ql/data/config/config.yaml
```

多用户需要注意，配置文件的名字必须以```mhy_```开头，之后的```[config*]```可以为任意字符

```sh
cp /ql/data/repo/ShanshanHY/MihoyoBBSTools-S/config/config.yaml.example /ql/data/config/mhy_[config1].yaml
cp /ql/data/repo/ShanshanHY/MihoyoBBSTools-S/config/config.yaml.example /ql/data/config/mhy_[config2].yaml
cp /ql/data/repo/ShanshanHY/MihoyoBBSTools-S/config/config.yaml.example /ql/data/config/mhy_[config3].yaml
……
cp /ql/data/repo/ShanshanHY/MihoyoBBSTools-S/config/config.yaml.example /ql/data/config/mhy_[config*].yaml
```

### 4.添加依赖

在青龙面板依赖管理中添加 httpx 及 PyYAML

### 5.编辑配置文件

单用户在配置文件内 config.yaml 中编辑信息

多用户在配置文件内 mhy_[config*].yaml 中编辑信息

_***注：通知配置为青龙 config.sh 中配置***_

## 使用 Docker 运行

Docker 的运行脚本基于 Linux 平台编写，暂未在 Win 平台测试。

将本项目 Clone 至本地后，请先按照上述步骤添加或修改配置文件。随后执行

```text
docker-compose up -d
```

启动 docker 容器。  
&nbsp;  
容器运行成功后可用

```text
docker-compose logs -f
```

命令来查看程序输出。

若需要添加配置文件或修改配置文件，可直接在主机 config 文件夹中修改，修改的内容将实时同步在容器中。

每次运行 Docker 容器后，容器内将自动按照参数执行签到活动，签到完成后容器将默认在每天上午 9:30 运行一次，如果想自行修改时间可自行编辑`docker-compose.yml`文件中的`CRON_SIGNIN`，将其修改成想运行的时间。

若想要更新容器镜像，可以参考以下命令

```text
docker-compose stop
docker-compose pull && docker-compose up -d
```

## 使用云函数运行

阿里云和腾讯云的云函数功能现已收费，请各位注意费用！

- 腾讯云

1. 下载本项目

2. 在脚本目录执行`pip3 install -r requirements_qcloud.txt -t .`

3. 在本地完整运行一次。

4. 打开并登录[云函数控制台](https://console.cloud.tencent.com/scf/list)。

5. 新建云函数 - 自定义创建，函数类型选`事件函数`，部署方式选`代码部署`，运行环境选 `Python3.6`.

6. 提交方法选`本地上传文件夹`，并在下方的函数代码处上传整个项目文件夹。

7. 执行方法填写 `index.main_handler`,多用户请填写`index.main_handler_mulit`.

8. 展开高级配置，将执行超时时间修改为 `300 秒`，其他保持默认。

9. 展开触发器配置，选中自定义创建，触发周期选择`自定义触发周期`，并填写表达式`0 0 10 * * * *`（此处为每天上午 10 时运行一次，可以自行修改）

10. 完成，enjoy it！

- 阿里云
  1. 下载本项目
  2. 在脚本目录执行`pip3 install -r requirements.txt -t .`，如果无法选择`Python3.9`环境请执行`pip3 install -r requirements_qcloud.txt -t .`
  3. 在本地完整运行一次。
  4. 打开并登录[函数计算 FC](https://fcnext.console.aliyun.com/cn-hangzhou/services)。注意左上方显示的地区，可点击切换其他地区。
  5. 创建服务 （日志功能可能产生费用，建议关闭）
     1. 创建函数
     2. 从零开始创建
        1. `请求处理程序类型：处理事件请求`
        2. 推荐设置运行环境为`Python3.9`
        3. `请求处理程序：index.main_handler`，多用户请填写`index.main_handler_mulit`
        4. 配置触发器：触发器类型 定时触发器 异步调用。建议触发方式设为`指定时间`
        5. 点击创建
     3. 进入函数详情
        1. 打开函数配置
        2. 修改 `环境信息` - `执行超时时间` 为 300 秒。
  6. 测试运行
     1. 打开 `函数详情`
     2. 点击`测试函数`
  7. 完成

## 更多方法，请融会贯通（
以上教程如有错误，还望海涵
