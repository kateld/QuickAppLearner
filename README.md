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