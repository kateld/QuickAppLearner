# QuickAppLearner

## 环境配置

quick app需要nodejs环境。

- 需安装6.0以上、8.0以下版本的nodejs，请从NodeJS官网下载，推荐v6.11.3 LTS。
- 建议先安装nvm，用nvm来管理nodejs版本，尤其是在正式工作环境下。

### 安装nvm

nvm的全称是Node Version Manager，一套使用广泛的nodejs版本管理工具。安装一般用以下两种方式即可：

通过curl:

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

通过Wget:

    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

建议在linux下工作，一般不会有什么其他安装问题，安装完毕后重启terminal之后基本就可以了。本教程全部基于ubuntu，其他安装方式参见[这里](https://github.com/creationix/nvm#install-script)

安装完毕后可以输入`nvm`确认一下是否安装成功。

安装完之后需要将nvm源修改为taobao源，在`.bashrc`末尾加入：

    NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node

保存，重开terminal，就可以开始安装nodejs了。

    nvm install v6.11.3 LTS

安装完试一下`node -v`输出是否正常，输出版本号说明安装完成了。

### 安装hap-toolkit

npm是nodejs的包管理工具，然而这里我们还是需要把默认源改成taobao源，不然很慢。这里直接安装taobao提供的cnpm工具，替代npm：

    npm install -g cnpm --registry=https://registry.npm.taobao.org

安装完成之后我们后续的npm命令都可以改用cnpm执行。接下来可以来装hap-toolkit：

    cnpm install -g hap-toolkit

在命令行中执行hap -V会输出版本信息表示hap-toolkit安装成功，如下命令所示：

    hap -V

## 'hello world!'

接下来我们来看看怎么安装运行快应用。

### 创建项目

如果我想创建的项目名叫project1：

    hap init project1

这样hap会在当前目录下创建project1文件夹并做一些基本的配置。结构如下：

    ├── node_modules
    ├── sign                      rpk包签名模块
    │   └── debug                 调试环境
    │       ├── certificate.pem   证书文件
    │       └── private.pem       私钥文件
    ├── src
    │   ├── Common                公用的资源文件和组件文件
    │   │   └── logo.png          manifest.json中配置的icon
    │   ├── Demo                  页面目录
    │   |   └── index.ux          页面文件，文件名不必与父文件夹相同
    │   ├── app.ux                APP文件（用于包括公用资源）
    │   └── manifest.json         项目配置文件（如：应用描述、接口申明、页面路由等）
    └── package.json              定义项目需要的各种模块及配置信息，npm install根据这个配置文件，自动下载所需的运行和开发环境

### 编译项目

在初始化之后，这个项目已经具备了在手机上运行的能力。在你的项目目录下执行：

    cnpm install

这个命令会为你的quickapp安装相关的依赖包，安装完毕后我们可以开始编译：

    cnpm run build

此时会生成两个文件夹：

- build：临时产出，包含编译后的页面js，图片等
- dist：最终产出，包含rpk文件。其实是将build目录下的资源打包压缩为一个文件，后缀名为rpk，这个rpk文件就是项目编译后的最终产出

一般来说我们只需要dist里的rpk。

### 安装调试器

现在quickapp还处于一个比较初步的阶段，要调试的话我们需要安装调试器及平台。因为迭代速度比较快，要获取最新的安装方式请移步[这里](https://doc.quickapp.cn/tutorial/getting-started/hello-world.html)

按照上面的提示安装rpk，就可以在手机上看到效果了。

## 项目结构

### manifest

与传统安卓apk一样，这里会对整个应用做一些全局配置，但内容比较不同。个人觉得这里的配置更像一个web应用，会有标准的路由将操作引导到页面。

这里直接用一个manifest文件来展开：

    {
        # 包名，区分不同应用的唯一id，因为名称其实是可以一样的
        "package": "com.application.demo",

        # 应用名称
        "name": "hi",

        # 版本管理的话，每次更新将versionCode自增1即可
        "versionName": "1.0.0",
        "versionCode": "1",
        "minPlatformVersion": "101",

        # 程序的入口icon，所有关于文件的引用统一使用根目录
        # 根目录对应src文件夹
        "icon": "/Common/logo.png",

        # 
        "features": [
            { "name": "system.prompt" },
            { "name": "system.router" },
            { "name": "system.shortcut" }
        ],

        "permissions": [
            { "origin": "*" }
        ],

        # 配置相关
        "config": {
            # 这里的设置是log输出的最低等级
            # 如果是warn的话，info类型将不会输出
            # 等级请参考js中的console日志
            "logLevel": "off"
        },

        # 路由
        # 这里会配置应用入口的页面
        # 所有的页面都需要在这里配置
        # 会把页面与对应的页面文件对应起来
        # 经过配置之后可以通过/Demo访问到Demo目录下的index.ux页面
        "router": {
            "entry": "Demo",
            "pages": {
                "Demo": {
                    # 这里对应的Demo文件夹里的index.ux
                    "component": "index"
                },
                "DemoDetail": {
                    "component": "index"
                },
                "About": {
                    "component": "index"
                }
            }
        },
        
        # 配置页面UI显示
        # 主要分为两种，页面公有与页面私有
        "display": {
            # 这三个都是所有页面公有的，顶部titleBar内容
            "titleBarText": "public title"
            "titleBarBackgroundColor": "#f2f2f2",
            "titleBarTextColor": "#414141",

            # 会增加一个导航栏
            "menu": true,

            # 页面私有内容
            "pages": {
                "Demo": {
                    # 这里面的内容就是每个页面私有的了
                    "titleBarText": "示例页",
                    "menu": false
                },
                "DemoDetail": {
                    "titleBarText": "详情页"
                },
                "About": {
                    "menu": false
                }
            }
        }
    }


... building