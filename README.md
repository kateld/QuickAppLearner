# QuickAppLearner

## 本文目的

- 结合官方文档，旨在让开发者更快了解、体验、开发QuickApp。
- 行文仓促，有bug恳请通过issue告知。
- 欢迎PR。

## 目录

- [快应用是什么](#快应用是什么)
- [环境配置](#环境配置)
- [第一个程序](#第一个程序)
- [项目配置](#项目配置)
- [框架](#框架)

## 快应用是什么

2018年3月20日，华为、小米、中兴、联想等手机厂商，共同举办了“快应用”标准启动发布会。

“快应用”是几家手机厂商基于硬件平台共同推出的新型应用生态，用户不必下载安装，即点即用，能够享受到原生应用的性能体验。“快应用”使用前端技术栈开发与原生渲染，兼具H5页面和原生应用的双重优点。

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

安装完之后需要将nvm源修改为taobao源，在`~/.bashrc`末尾加入：

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

## 第一个程序

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

现阶段如果要在其他手机上体验快应用，需要安装下面这两个apk，点击下载：

- [调试器](https://statres.quickapp.cn/quickapp/quickapp/201803/file/201803221213415527241.apk)
- [预览平台](https://statres.quickapp.cn/quickapp/quickapp/201803/file/20180322121456491785.apk)

运行方案推荐两种，http与本地。

- 本地
    - 直接把rpk文件拖入手机，在调试器里本地安装即可
- http
    - 实质上是在主机上运行一个服务器，通过http请求调试
    - 在主机上运行`npm run server`
    - 会出现一个二维码，在调试器上扫描即可

 ![image](https://github.com/williamfzc/QuickAppLearner/raw/master/pic/debuger.png)

之后就可以愉快的运行了，自测的效果感觉还可以。

![image](https://github.com/williamfzc/QuickAppLearner/blob/master/pic/example.png)

## 项目配置

项目配置在`src/manifest.json`中。

与传统安卓apk一样，这里会对整个应用做一些全局配置，但内容比较不同。个人觉得这里的配置更像一个web应用。

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

## 框架

### 生命周期

与android应用相同，快应用也具备类似的生命周期。但相对而言要简明许多。

- 页面的生命周期
    - onInit
    - onReady
    - onShow
    - onHide
    - onDestroy
    - onBackPress
    - onMenuPress
- 页面的状态：
    - 显示
    - 隐藏
    - 销毁
- APP的生命周期：
    - onCreate
    - onDestroy

那么这些东西要写在哪里？我们随便打开一个index.ux文件可以发现，一般来说一个页面由三个部分组成，分别是template、style和script。这部分的代码将被放置到script下。

    <template>
    <!-- template里只能有一个根节点 -->
    <div class="demo-page">
        <text class="title">{{text}}</text>
    </div>
    </template>

    <style>
    .demo-page {
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .title {
        font-size: 40px;
        text-align: center;
    }
    </style>

    <script>
    export default {
        data: {
            text: '欢迎打开详情页'
        },
        /**
        * 当用户点击菜单按钮时触发，调用app中定义的方法showMenu
        * 注意：使用加载器测试`创建桌面快捷方式`功能时，请先在`系统设置`中打开`应用加载器`的`桌面快捷方式`权限
        */
        onMenuPress() {
            this.$app.showMenu()
        }
    }
    </script>

这三个部分的内容分别是，html本人，css本人和js脚本部分。可以看到quickapp的内层基本都是跟web走的是同一套理论，也算是未来的一大趋势吧。

#### 页面生命周期

页面通过ViewModel渲染，所以页面的生命周期指的就是ViewModel的生命周期。

##### onInit

表示数据已经准备好，可以使用props、data中的数据。

    data: {
        // 生命周期的文本列表
        lcList: []
    },
    onInit () {
        // 可以调用lcList
        this.lcList.push('onInit')
    }

##### onReady

因为快应用是一种类似web应用的模式，页面会有相应的模板。这个方法表示模板加载已经完成，可以开始进行模板级别的操作例如嵌入数据。

    onReady () {
        console.info(${this.$rootElement()}`)
    }

##### onHide与onShow

- APP中可以同时运行多个页面，但是每次只能显示其中一个页面
- 页面被切换隐藏时调用onHide()，页面被切换重新显示时调用onShow()

code

    onShow () {
        console.info(`执行：获取页面显示状态属性：${this.$visible}`)  // true
    }
    onHide () {
        console.info(`执行：获取页面显示状态属性：${this.$visible}`)  // false
    }

##### onDestroy

页面被销毁时调用，被销毁的可能原因有：用户从当前页面返回到上一页，或者用户打开了太多的页面，框架自动销毁掉部分页面，避免占用资源。

要注意的是，页面销毁之后，绑定在上面的方法将不再执行。

##### onBackPress

三种情况下触发：

- 返回实体按键
- 左上角返回菜单
- 调用返回的API

##### onMenuPress

在manifest中允许菜单出现后，在点击展开菜单时会调用。

#### 页面状态

总的来说页面状态只有三种：

- 显示
    - 该页面是当前正在显示的页面，可以用$visible判断
- 隐藏
    - 打开新页面后该页面被隐藏，但未被销毁的页面
- 销毁
    - 因某原因销毁后页面将处于这个状态

#### APP状态

当前为APP的生命周期提供了两个回调函数：onCreate, onDestroy；可在app.ux中定义回调函数。在app.ux中，开发者可以做一些独立于页面的操作。比如：引入公共的JS资源，然后暴露给所有页面。

在不同ux中调用方法的方式是不同的。在app.ux中：

    console.info(`获取：APP文件中的数据：${this.$def.data1.name}`)
    console.info(`执行：APP文件中的方法`, this.$def.method1())
    console.info(`获取：manifest.json的应用名称：${this.$def.manifest.name}`)
    console.info(`获取：manifest.json的config.data的数据：${this.$data.name}`)

在pageName.ux中，通过this.$app访问app.ux中定义的数据和方法，如下：

    console.info(`获取：APP文件中的数据：${this.$app.$def.data1.name}`)
    console.info(`执行：APP文件中的方法`, this.$app.$def.method1())
    console.info(`获取：manifest.json的应用名称：${this.$app.$def.manifest.name}`)
    console.info(`获取：manifest.json的config.data的数据：${this.$app.$data.name}`)

理解起来也很容易，def是在app.ux中的对象，在app.ux内部调用自然直接this即可，在外部调用就需要通过`$app`中调用。另外，`$app`是确实存在的实体，而`$def`相当于它的一部分嵌入其中而已。

### 页面布局

在前面提到了，quickapp的页面都是跟web同一套渲染流程，只不过他将一些html标记映射成android的native组件了，渲染出来的东西不一样。那么要写页面的话，就需要在ux里先写好模板。虽然跟web很像，但实际上quickapp还是在html的基础上做了不少改变的。

传统html该有的他都有，css选择器那一套跟原来基本保持原样，这里不过多赘述。着重讲一下不同的地方。

#### for if show

快应用很神奇的在html里加入了语法...

先来看循环。在html里加入循环可以有效地解决批量渲染控件的问题。一般有三种使用语法，通过一段.ux代码来说：

    <template>
    <div class="tutorial-page">

        <!-- 方式1：默认$item代表数组中的元素, $idx代表数组中的索引 -->
        <div class="tutorial-row" for="{{list}}">
        <text>{{$idx}}.{{$item.name}}</text>
        </div>

        <!-- 方式2：自定义元素变量名称 -->
        <div class="tutorial-row" for="value in list">
        <text>{{$idx}}.{{value.name}}</text>
        </div>

        <!-- 方式3：自定义元素、索引的变量名称 -->
        <div class="tutorial-row" for="(personIndex, personItem) in list">
        <text>{{personIndex}}.{{personItem.name}}</text>
        </div>
    </div>
    </template>

    <style lang="less">
    .tutorial-page {
        flex-direction: column;

        .tutorial-row {
            width: 85%;
            margin-top: 10px;
            margin-bottom: 10px;
        }
    }
    </style>

    <script>
    export default {
        onInit () {
            this.$page.setTitleBar({ text: '指令for' })
        },
        # 这里的list就可以直接插入到html里了
        data: {
            list: [{name: 'aa'}, { name: 'bb' }]
        }
    }
    </script>

那么if跟show，这两个主要用于管理控件的出现与否。同样来一段.ux代码：

    <template>
    <div class="tutorial-page">
        <text onclick="onClickShow">显示隐藏：</text>
        <!-- show的功能是，可以根据值的不同让控件显示与消失，但它的消失并不会从DOM树移除 -->
        <text show="{{showVar}}">show: 渲染但控制是否显示</text>

        <!-- 这里可以看到，if的功能主要是用于控制应该渲染什么样的控件 -->
        <!-- 需要注意的是同一套if elif else的节点需要是兄弟节点 -->
        <text onclick="onClickCondition">条件指令：</text>
        <text if="{{conditionVar === 1}}">if: if条件</text>
        <text elif="{{conditionVar === 2}}">elif: elif条件</text>
        <text else>else: 其余</text>
    </div>
    </template>

    <style lang="less">
    .tutorial-page {
        flex-direction: column;
    }
    </style>

    <script>
    export default {
        onInit () {
        this.$page.setTitleBar({ text: '指令if与指令show' })
        },
        data: {
        showVar: true,
        conditionVar: 1
        },
        onClickShow () {
        this.showVar = !this.showVar
        },
        onClickCondition () {
        this.conditionVar = ++this.conditionVar % 3
        }
    }
    </script>

#### block slot

显而易见的，我们如果在这里面把逻辑都写在一起，代码将变得异常混乱。好在我们还有block组件。block组件在quickapp上主要承担逻辑分块的作用，因为它没有对应的native组件，所以它是不会被渲染到页面上的。所以用它来分隔代码块是很合适的。

    <template>
    <div class="tutorial-page">
        <text onclick="toggleCityList">点击：控制是否显示城市</text>
        <list>
        <block for="city in cityList" if="{{showCityList === 1}}">
            <list-item type="city">
            <text>城市：{{city.name}}</text>
            <block for="{{city.spots}}">
                <div show="{{city.showSpots}}">
                <text>景点：{{$item.name}}</text>
                </div>
            </block>
            </list-item>
        </block>
        </list>
    </div>
    </template>

看起来很乱对吧，没关系，我们还可以把他们拆分到不同文件里去，把整个页面拆分成若干子组件，最后再进行拼接渲染得到整个页面。

slot节点用于向开发者额外开发的自定义ux组件中插入内容。例如我们自定义组件part1.ux：

    <!-- part1.ux -->
    <template>
    <div>
        <text>{{ header }}</text>
        <!-- 这里用了slot -->
        <slot></slot>
        <text>{{ footer }}</text>
    </div>
    </template>

    <style>
    </style>

    <script>
    export default {
        props: [
            'header', 'footer'
        ]
    }
    </script>

然后我们在index.ux里导入它

    <!-- index.ux -->
    <import src="./part1"></import>

    <template>
        <!-- 这里就可以根据需要定义slot部分的内容 -->
        <!-- 同时，这里就是把part1的template部分给导进来了 -->
        <part1 class="component" header="{{header}}" footer="{{footer}}">
            <text>slot节点内容</text>
        </part1>
    </template>

    <style>
    .component {
        flex-direction: column;
    }
    </style>

    <script>
    export default {
        onInit () {
            this.$page.setTitleBar({ text: '组件slot' })
        },
        data: {
            'header': 'HEAD',
            'footer': 'FOOT'
        }
    }
    </script>

最终效果就是，index.ux也会有part1.ux里的组件。这种设计方案是非常推荐的，非常方便后期的管理与逻辑分块。这部分后面会细讲。

### 页面切换和参数传递

本部分主要四个内容：

- 通过组件a切换页面和传递参数
- 通过接口router切换页面和传递参数
- 接收参数
- 回传参数

#### a

a在html中主要起到跳转作用，在quickapp也是一样的。

    <template>
    <div class="tutorial-page">
        <!-- 添加参数 -->
        <!-- 这里跳转是根据路由的设定来的，基本就跟web一样 -->
        <a href="/PageParams/receiveparams?key=Hello, world!">携带参数key1跳转</a>
        <!-- 添加变量参数 -->
        <a href="/PageParams/receiveparams?key={{title}}">携带参数key2跳转</a>
    </div>
    </template>

他还可以唤起其他app的功能，例如拨打电话、sms、打开网页等等

    <template>
    <div class="tutorial-page">
        <!-- 包含完整schema的uri -->
        <a href="tel:10086">调起电话</a>
        <a href="sms:10086">调起短信</a>
        <a href="mailto:example@xx.com">调起邮件</a>
        <!-- 打开webview加载网页 -->
        <a href="https://www.baidu.com/">打开网页</a>
    </div>
    </template>

#### router

当然，直接写在a里面好像有些不灵活，那么我们也可以用类似android原生的控件监听，在script里管理跳转，此时控件只需要绑定对应的函数。

    <template>
    <div class="tutorial-page">
        <input class="btn" type="button" value="跳转到接收参数页面" onclick="routePagePush"></input>
        <input class="btn" type="button" value="跳转到接收参数页面，当前页面无法返回" onclick="routePageReplace"></input>
        <input class="btn" type="button" value="返回上一页" onclick="routePageBack"></input>
        <input class="btn" type="button" value="清空页面记录，仅保留当前页面" onclick="routePageClear"></input>
    </div>
    </template>

    <script>
    // 导入模块
    import router from '@system.router'

    export default {
        onInit () {
            this.$page.setTitleBar({ text: '接口router切换页面' })
        },
        routePagePush () {
            // 跳转到应用内的某个页面
            router.push({
                uri: '/PageParams/receiveparams'
            })
            // 传递参数
            params: { key: this.title }
        },
        routePageReplace () {
            // 跳转到应用内的某个页面，当前页面无法返回
            router.replace({
                uri: '/PageParams/receiveparams'
            })
        },
        routePageBack () {
            // 返回上一页面
            router.back()
        },
        routePageClear () {
            // 清空所有历史页面记录，仅保留当前页面
            router.clear()
        }
    }
    </script>

#### 接收参数

接收参数就很简单了，只需要在data里面声明好就可以用了。

    <script>
    export default {
        data: {
            key: ''
        },
        onInit () {
            this.$page.setTitleBar({ text: '接收参数' })

            // js中输出页面传递的参数
            console.info('key: ' + this.key)
        }
    }
    </script>

#### 回调函数

在开发中我们可能会遇到先从页面A跳转至页面B，然后从页面B返回到页面A时，需要传递参数的情况。这种情况下比较麻烦，一般靠APP级别的全局参数来解决。

页面A

    <script>
    export default {
        data: {
            msg: ''
        },
        onInit () {
            this.$page.setTitleBar({ text: '页面A' })
        },
        onShow () {
            // 页面被切换显示时，从global数据中检查是否有页面B传递来的数据
            if (this.$app.$data.dataPageB && this.$app.$data.dataPageB.gotoPage === 'pageA') {
                // 从global数据中获取回传给本页面的数据
                var data = this.$app.$data.dataPageB.params;
                this.msg = data.msg;
            }
        }
    }
    </script>

页面B

    <script>
    export default {
        data: {
            msg: ''
        },
        onInit () {
            this.$page.setTitleBar({ text: '页面B' })
        },
        onHide () {
            // 页面被切换隐藏时，将要传递的数据对象写入global数据
            this.$app.$data.dataPageB = {
                gotoPage: 'pageA',
                params: {
                msg: this.msg
                }
            }
        },
    }
    </script>



... building