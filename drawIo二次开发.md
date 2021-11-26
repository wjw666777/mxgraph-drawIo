## 参考文档

[drawio 官方文档](https://link.zhihu.com/?target=https%3A//www.diagrams.net/doc/)

[drawio 二次开发优秀项目](https://link.zhihu.com/?target=http%3A//processflow.sharehoo.cn/)

[drawio 编码和解密工具](https://link.zhihu.com/?target=https%3A//jgraph.github.io/drawio-tools/tools/convert.html)

[drawio 使用教程](https://zhuanlan.zhihu.com/p/129659662)

[drawio 画流程图](https://link.zhihu.com/?target=https%3A//blog.csdn.net/zyx_ly/article/details/102488457)



drawIo二次开发





既然是基于 DrawIO 进行二开，首先就来了解一下 DrawIO 到底是什么，跟着笔者视角先去 [DrawIO 官网](https://link.zhihu.com/?target=https%3A//www.diagrams.net/)去看一圈，官网首页截图如下所示：

![img](https://pic4.zhimg.com/v2-94d26321e58a22ccf005540afc2d618b_b.jpg)

简要来说**DrawIO 是一个支持绘制各种形式图表并且支持多位置存储能力的网站和工具**。绘制图表的功能就不多说了，这也是 DrawIO 自身的核心功能，这里着重介绍一下 DrawIO 的多位置存储能力，通过 DrawIO 绘制的图表可以存在网络上的很多地方，比如：浏览器缓存、本机文件夹、远程存储（Github、Gitlab 和 Google Drive 等）。存储浏览器和本机文件夹很简单不做介绍，这里以 Github 为例简单讲解一下通过 DrawIO 绘制的图表如何进行远程存储。

### **存储 DrawIO 到 Github 仓库**

- 1 - 在 Github 新建一个仓库

这里笔者新建了一个名为 [drawio-folder](https://link.zhihu.com/?target=https%3A//github.com/luffyZh/drawio-folder) 的仓库，里面只有一个 `README.md` 文件，如下图所示：

![img](https://pic4.zhimg.com/v2-849ecc2b112431553c97e7dc4d8b03f3_b.jpg)

- 2 - 绘图时选择 GitHub 作为存储空间

然后从官网点击 Start 按钮进入绘图页面，第一次进入页面不出意外会出现一个如下图所示选择存储空间的选项，然后选择 Github（其他远程存储原理一样）。

![img](https://pic1.zhimg.com/v2-e1c7e7893a4102741e76b5540ce5394c_b.jpg)

- 3 - 选择对应的仓库然后绘制图片

点击选择存储到 Github 仓库，然后选择新建一个图表，命名之后点击新建 Create 按钮完成图表的创建。

![img](https://pic3.zhimg.com/v2-a0e09c81909fe85174dc656dfe80964a_b.jpg)

- 4 - 授权选取存储仓库

图标创建成功后，会出现一个授权登录的弹窗，授权登录过后 DrawIO 就能获取到账户下的所有仓库信息，选择刚才创建的 **drawio-folder** 仓库，然后进行图表的绘制。

![img](https://pic3.zhimg.com/v2-45e8cc7bcffaeca717c3b819ee9a7d4a_b.jpg)

- 5 - 绘图完毕保存文档

绘制过程中，每一次的保存操作，都会创建一条 `commit message`，就相当于正常的仓库修改并提交，如下图所示：

![img](https://pic2.zhimg.com/v2-1785a17906c1680f2616e7fa7ac0d301_b.jpg)

当一次图表绘制完成并保存后，可以去 Github 仓库看看是否存储成功。

![img](https://pic1.zhimg.com/v2-6e70a39a83b52df44114094591c733ac_b.jpg)

可以看到，刚才创建的图表已经成功存储到了 Github 仓库，非常的便捷，基本上就等于你已经有了一个无限制的图表云盘一样。

至此相信大家应该已经了解了 DrawIO 如何绘图以及它强大的存储功能，在文章的开头提到过，如果你是一个单纯的使用者，那么文章读到这里已经足够了，你已经拥有了一个免费且强大的绘图工具（不需要开通会员不需要注册也没有绘制数限制）以及很多类型的无限存储空间，相比还要收费的 Some Apps 它不香吗？

### **为什么要进行 DrawIO 二开**

既然 DrawIO 本身已经这么香了，为啥还要进行二开呢？在这简单总结几点为什么要进行二开或者可能需要进行二开的原因：

- 首先，对于一个团队来说需要的不仅仅是一个纯绘图工具，而是一个包含绘图功能的一体化管理平台，绘图只是其中最核心的部分，其余还有一些必不可少的功能。
- 其次，在一些特殊业务需求中，需要把图表和自身的业务结合起来以发挥图表更大的能力或者基于业务进行特殊改造，这种场景的话二开就是必然的。
- 最后，就是所谓的逼格问题，直白点说就是能放到自己公司独立部署服务的，为啥放在外面？免费版万一有一天不免费了或者有限制了那就被动了。（虽然可能性极低，但是也要考虑这种因素）

>  如果你或者你的团队也有上面几点原因，那么欢迎继续往下看，接下来会介绍如何以 DrawIO 官方开源代码为基础进行简单的二次开发。

## **DrawIO 二开 —— 流程准备**

> **【注意】**：本文主要针对前端开发者，但是在开发部署构建新版本的过程有很多依赖 Java 环境的内容，所以为了保证文章的连贯性，也会进行简单介绍，如果你已经熟悉相关环境配置，那么忽略对应内容即可。

### **环境准备**

- 前端开发环境

这里就不多说啥了，其实 DrawIO 的开发还真就是完全属于前端范畴，所以各位前端同学如果遇到类似的需求，就迎难而上吧～

- 编译构建相关的环境 —— Java 和 Ant

>  关于 Java 和 Ant 两个环境的安装，这边以 MacOS 为例进行讲解，其他系统参考官方教程。

至于为啥要用到这俩，后面构建部署过程会介绍到，因为前端开发同学的电脑里大概率不会安装这俩，所以这里也简单讲一下安装步骤。

```text
# jdk 下载地址
https://www.oracle.com/java/technologies/javase-jdk16-downloads.htmlt

# ant 下载地址
https://ant.apache.org/bindownload.cgi
```

对于 jdk 安装包下载完之后正常安装即可，检验是否安装成功可以使用 `javac` 或者 `java -version` 命令来查看安装版本，之后将下载的 Ant 压缩包解压到 Applications 目录下，然后运行如下命令。

```text
# 写入环境变量
sudo vim .bash_profile
# 这里注意，版本号和你自己下载的要对应上
export ANT_HOME=/Applications/apache-ant-1.9.16
export PATH=$ANT_HOME/bin:$PATH
# 退出并保存
:wq
# 让环境变量生效
source .bash_profile
# 查看版本
ant -V
```

![img](https://pic2.zhimg.com/v2-6028281e3ddc71bcdd6da747b5566105_b.jpg)

安装完成之后可以在命令行验证是否安装成功，如果命令行里出现了如上图所示对应的 Java 和 Ant 版本信息，那么恭喜你 DrawIO 二开的基础环境已经准备完成，接下来就是正式进入 DrawIO 二开了。

### **DrawIO 源码**

二开的开始，首先从 [DrawIO 源码仓库](https://link.zhihu.com/?target=https%3A//github.com/jgraph/drawio) Clone 官方代码，在本地跑一下看看效果。老规矩 Clone 下来先阅读 `package.json` 文件，发现其中只有一个 `start` 命令。

```text
"scripts": {
  "start": "electron ."
}
```

了解前端的同学应该能知道，此命令将会启动了一个 Electron 桌面应用，运行此命令过后不出意外的，一个桌面版 DrawIO 就呈现在眼前了。

![img](https://pic1.zhimg.com/v2-0c6730b61633d20c0ca4d681e38e0170_b.jpg)

它的使用方式和上面文章里介绍的一样，但是问题来了！我们最终目标是要进行 DrawIO 二开，难不成是基于 Electron 二开吗？虽然说也不是不行，但是那上手成本可就高了，估计会劝退一大波同学，不要慌，去官方仓库看看有没有其他方法。

![img](https://pic4.zhimg.com/v2-4b75206f4131c8dbdc3c757cebef2a2b_b.jpg)

功夫不负苦心人，如上图官方文档介绍了除了基于 Electron 部署应用外，DrawIO 还能以静态站的形式部署到 Github Pages，并且官方的站点地址点开后就更清晰了：`https://jgraph.github.io/drawio/src/main/webapp/index.html`。从链接可以观察到其实 Github Pages 部署的站点就是 `/src/main/webapp` 这个文件夹，也就是 DrawIO 的静态站根目录，那么静态站的开发对于前端来说再熟悉不过了，所以接下来所有的二开过程都是基于静态站的形式进行讲解。

>  如果大家目标就是基于 Electron 构建桌面端应用，也可以对应的进行相应的二开，流程上和静态站的二开基本一致。开发模式的选型主要是通过分析自己团队的业务诉求得出结论，因为网页类应用普适性比较好并且没有那么多局限性，所以云音乐选择的就是静态站的模式。

### **开发模式及预览**

一个陌生的应用系统或者框架怎么熟悉最快，最简单的办法就是顺着入口文件一点一点的啃下去了，这与平时阅读源码的过程很类似，DrawIO 静态站的入口文件是 `/src/main/webapp/index.html`，接下来从它入手，简单介绍一下如何在本地开发调试 DrawIO 代码。

- 第一步：新建 `package.json`

```text
{
  "name": "drawio-dev",
  "version": "1.0.0",
  "main": "src/main/webapp/index.html",
  "scripts": {
    "start": "cd src/main/webapp && serve"
  },
  "license": "MIT",
  "dependencies": {
    "serve": "^12.0.0"
  }
}
```

首先，根目录新建一个 `package.json` 文件，然后键入如上内容，然后通过 `yarn start` 或者 `npm run start` 命令，就能很简单的在本地启动一个服务器，进行开发访问，效果如下：

![img](https://pic1.zhimg.com/v2-f249b749385314f8a3a2433152fb20ec_b.jpg)

- 第二步：修改代码，开发模式预览

由于 DrawIO 项目的文件体系十分的庞大，这里不做过多介绍，主要给大家介绍一下二开过程中核心的一个文件目录 `src/main/webapp/js/diagramly`，此文件夹内的文件是系统运行最为核心的部分，二开的工作大部分也都是修改此文件夹的文件内容。

其中，`src/main/webapp/js/diagramly/App.js` 算是加载 DrawIO 的一个入口 js 文件，我们在其中第一行键入一段代码：

![img](https://pic1.zhimg.com/v2-3d92062086e647e4fc53efbc524b3784_b.jpg)

很容易理解，就是 Alert 一个弹窗提示测试一下代码是不是能正常执行，不过刷新页面之后会发现并没有看到这个弹窗，这是为什么呢？打开控制台找找原因：

![img](https://pic4.zhimg.com/v2-29b79f0979a6d0116a9103eb49268903_b.jpg)

发现加载的不是 `app.js` 而是 `app.min.js`，看来默认加载的是生产版本的代码，那么如何执行开发代码进入开发模式呢？仔细阅读源码会发现，DrawIO 是通过识别 url query 的 `dev` 参数来判断是不是开发模式（`dev === 1` 则为开发模式），那么加上这个参数再来试试：

![img](https://pic2.zhimg.com/v2-3ada001f9b1fc6006aa381c3eb592e01_b.jpg)

到这里，笔者内心莫名出现了几万头不知名的可爱动物，果然没有这么容易啊。没事，技术就是一个不断探索的过程，希望各位看官能和我一样不要放弃。

> **当然，看完本篇文章你们就已经少走了 90% 的弯路，直接进行二开就可以了。**

为啥会找不到文件呢？简单分析一下，还是文件的指向问题：

![img](https://pic4.zhimg.com/v2-ca51bcaaf92984005a81b35d9f634987_b.jpg)

在本地开发应该请求的是本地的资源文件，但是 Network 面板发现请求的资源地址并不对，废话不多说，再说下去可能大家内心也崩溃了，通过如下代码，把开发模式安排明白。

```text
// index.html 从 245 行开始

// Changes paths for local development environment
if (urlParams['dev'] == '1') {
    ...

  // ====> 开发模式文件指向本地
  if (location.hostname == 'localhost' || location.hostname == '127.0.0.1') {
    drawDevUrl = `http://${location.host}/`;
    geBasePath = `http://${location.host}/js/grapheditor`;
    mxBasePath = `http://${location.host}/mxgraph`;
    mxForceIncludes = true;
  }
  // ====> 开发模式文件指向本地

  mxscript(drawDevUrl + 'js/PreConfig.js');
  mxscript(drawDevUrl + 'js/diagramly/Init.js');
  mxscript(geBasePath + '/Init.js');
  mxscript(mxBasePath + '/mxClient.js');
  ...
}
```

核心代码以及对应位置就在上面已经给出了，修改过后直接刷新页面看一下效果：

![img](https://pic2.zhimg.com/v2-553c6a9e7adaf591f782850a7a88e7d9_b.jpg)

可以看到，我们心心念念的 Alert 弹窗已经出来了，此时此刻，笔者已经是老泪纵横了，到这里为止，DrawIO 二开的开发流程基本介绍完成，关于二开的更多细节如何去改造 DrawIO 就是各位根据自身团队的需求而定了。

### **构建生产版本**

上面搞定了开发模式，完成了本地开发实时预览，但是最终发布的代码肯定还是要用生产版本，前面也知道了，生产版本使用的是诸如 `app.min.js` 这种压缩文件，那么如何把本地开发的代码打包编译为生产版本呢？这就要用到前面已经安装好的 Java 和 Ant 环境了。

这里没有过多内容，非常简单的在 `package.json` 文件里增加如下代码即可：

```text
{
  "name": "drawio-dev",
  "version": "1.0.0",
  "main": "src/main/webapp/index.html",
  "scripts": {
    "start": "cd src/main/webapp && serve",
+   "build": "cd etc/build && ant"
  },
  "license": "MIT",
  "dependencies": {
    "serve": "^12.0.0"
  }
}
```

增加完上述 `build` 命令之后运行一下，不出意外会出现如下构建成功的提示：

![img](https://pic3.zhimg.com/v2-83e8347b33ecfd1dc1f631407f81316a_b.jpg)

然后再来访问一下首页，这次不携带 `dev=1` 开发参数看看效果，如下图所示，非常完美的运行着新开发的生产版本，至此为止 DrawIO 的二开流程阶段，从开发预览到构件生产版本都已经介绍完成。

![img](https://pic3.zhimg.com/v2-4a0933e4c57f73cdb76661952530086a_b.jpg)

### **部署**

部署的话，其实就参考静态站部署就可以了，核心目录就是上面提到过的 `src/main/webapp`。笔者在这里简单给各位提供几个部署思路：

- Github Pages(个人)
- Vercel(个人)
- Nginx(团队)
- Node Server(团队)

至于如何部署，各位根据自身或者团队实际情况进行选择即可。

## **DrawIO 二开 —— 技术细节**

笔者基于 DrawIO 开发的绘图管理系统在云音乐团队内部内测中，目前来说已完成的和正在开发中的功能集如下：

*√*基础图绘制功能

*√*基于云音乐内部服务的增删改查

*√*个人中心模块，管理自己的图表内容

*√*数据交互，图表与业务系统进行数据交互，一键导入已创建的图表

*x*Todo - 鉴权功能，比如分享，指定权限的人可以进行相应操作 

*x*Todo - 团队空间，一个团队可以管理一个图表的集合

*x*Todo - 版本历史，支持查看最近几次的版本修改内容

*x*Todo - 自定义导入一些基于公司/个人比较常用的模板

由于本文还是偏技术向的文章，上面很多功能其实和 DrawIO 二开没啥太大的关系，都是一些基于业务的需求，所以不做过多细节介绍，笔者相信各位开发者/团队进行 DrawIO 二开的主要原因，80% 以上都是因为想要把内容保存到内部服务上，也就是说获取当前画板并按照一定的形式保存在数据库里，这才是整个二开过程中最为核心的部分。这里以一个二开过程中的源码案例扩展讲一下 DrawIO 二开过程中的部分技术细节。

### **获取当前画板内容 —— `getCurrentFile`**

那么如何获取当前画板内容呢？还是通过对 `App.js` 源码的阅读，我发现了如下这段代码：

```text
App.prototype.save = function(name, done) {
    var file = this.getCurrentFile();
  ...
}
```

顾名思义，这段代码会在保存的时候执行，并且会获取到一个 file 文件对象，添加一个 log 后运行一下代码，看看效果：

![img](https://pic4.zhimg.com/v2-1276bbd29759a59013c77d0f6b187507_b.jpg)

可以看到，确实打印出来了一个 file 文件对象，关于对象里面的众多属性这里不一一说明，只介绍一个最核心的 `data` 属性也就是文件数据，有了这个文件数据能做什么呢？别着急，接着往下看。

### **构造文件下载到本地**

通过 `getCurrentFile` 我们从画布获取到了当前正在绘制的图表数据，看内容是一段 xml 字符串，接下来就通过这个字符串反向构造一个文件下载下来，看看是不是和画布上的那个图表一模一样。此处通过 `filesaver` 将构造的文件数据进行下载，核心代码如下：

```text
/**
 * 构造文件下载到浏览器
 **/
function download() {
  const fileObj = {
    title: 'luffyzh.drawio',
    data: '<mxfile host="localhost" modified="2021-09-02T09:50:39.574Z" agent="5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36" etag="PyBU88KGLdBE5FU7fpPf" version="@DRAWIO-VERSION@" type="device"><diagram id="C5RBs43oDa-KdzZeNtuy" name="Page-1">7VhZc5swEP41PLbD4WDnsThx0tbNpE3aTB9lkIVqwVIhfOTXdzHCgPH4aOscnvjBo12WlbTft4dtOP1ofiVJEn6BgArDNoO54VwYtn3WNfE7VywKhduzCwWTPChUVqW4449UK/V7LOMBTRuGCkAonjSVPsQx9VVDR6SEWdNsDKK5a0IYbSnufCLa2gceqLDQ9uxupb+mnIXlzpZ7XjyJSGmsb5KGJIBZTeVcGk5fAqhiFc37VOSxK+Py8HHxIIYT9+rT1/Q3+e59vr/58a5wNjjkldUVJI3V/3WtsZwSkel46buqRRlACVkc0NyJaTheqCKBSwuXv6hSCw04yRSgCqQKgUFMxBAg0XZjiJU2s3KZxsGHHFiURwL8SaEacCH0Hihp+x5KqZIwWWGXO1gBkRsLMqLCI/6ELQ/aBwESH8UQ09xVgGTQd6kOd1lpvT1jqzFIIZM+3WLnaIoTyeg2f25hl5+vxlON3BWFiCq5QANJBVF82iQz0TnBVnYV7rjQ0B9AA6dFA5GNx4vHcAsbciRmIVf0LiHLkMywfjQZUkceL+cxQdJU47YD1sNgmVKp6HxrIMunrs5mXc56WpxVtcEqEz6s1YWOeaTQd1qh/4nl8i0J/yUJ3T2TsOxRO7NQk6Ukxt5JqT3dAsd7VSYwHqd4sHXqrDb8ezadtdh0Ay0y7cbjVdPtmcjUPYxL1ovnktvi0pBEiWG7AsPgjSSuWL5KRMYYkgXdxYYzaNeuEKJRlu5uFw2McwYNSMRFHq9rKqZUcZ9saCpEcIb7XvgIN5WbyYNb8pih5FbS/ZKsWH+P2Gy6zWazkuvdxtzQbXrH6jbdFqa3iF4BnW2KJb4n1vEdew2E8+du+b19ivSrLsFP3vHLarq75Vt7lmnNHvO9g58GgV7+FHC+10z5NgYch2H2qQ0C5QlrfPIyMWpPAqNMxstBADL1NglsmwTsTU3oSSeBshDWQP1GE8IRTHN4inPA+i//jvPcc0BZKZoQiDymtllk2IlhsD6LOdbxMECx+m+2KIXVH9zO5R8=</diagram></mxfile>'
  }
  const blob = new Blob([fileObj.data], {type: 'application/octet-stream'});
  saveAs(blob, fileObj.title);
}
```

从代码和截图效果可以得出结论：通过 `getCurrentFile` 获取到图表核心数据 data，就能很容易的反向构造出来一个 .drawio 文件也就是绘图文件，并且和画布上的文件一模一样（感觉自己好像在说废话，当然一模一样了，数据都是一样的他不一样可就奇怪了）。

### **将保存的图表数据绘制到画布上 —— `loadFile`**

上面的两步我们确定了画布数据的获取以及数据是否能反向构造出来图表文件，由此确定了技术方案的可行性，但是其实还差最后一个环节 —— 加载图表数据到画布上，因为你的图表不仅仅能保存，还需要能继续编辑，因此把数据从文件里获取出来然后反向构造成图表数据并加载到画布上，这才完成了一个 DrawIO 二开的最小闭环。这里不做过多废话，核心 API — `loadFile`，细节代码如下：

```text
if (this.editor.isChromelessView()) {
    ...
}
/**
 * 如果有id，表示打开了一个存在的文档，进入编辑模式
 */
else if (urlParams['id']) {
  const mockFile = {
    title: 'luffyzh.drawio',
    data: '<mxfile host="localhost" modified="2021-09-02T09:50:39.574Z" agent="5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36" etag="PyBU88KGLdBE5FU7fpPf" version="@DRAWIO-VERSION@" type="device"><diagram id="C5RBs43oDa-KdzZeNtuy" name="Page-1">7VhZc5swEP41PLbD4WDnsThx0tbNpE3aTB9lkIVqwVIhfOTXdzHCgPH4aOscnvjBo12WlbTft4dtOP1ofiVJEn6BgArDNoO54VwYtn3WNfE7VywKhduzCwWTPChUVqW4449UK/V7LOMBTRuGCkAonjSVPsQx9VVDR6SEWdNsDKK5a0IYbSnufCLa2gceqLDQ9uxupb+mnIXlzpZ7XjyJSGmsb5KGJIBZTeVcGk5fAqhiFc37VOSxK+Py8HHxIIYT9+rT1/Q3+e59vr/58a5wNjjkldUVJI3V/3WtsZwSkel46buqRRlACVkc0NyJaTheqCKBSwuXv6hSCw04yRSgCqQKgUFMxBAg0XZjiJU2s3KZxsGHHFiURwL8SaEacCH0Hihp+x5KqZIwWWGXO1gBkRsLMqLCI/6ELQ/aBwESH8UQ09xVgGTQd6kOd1lpvT1jqzFIIZM+3WLnaIoTyeg2f25hl5+vxlON3BWFiCq5QANJBVF82iQz0TnBVnYV7rjQ0B9AA6dFA5GNx4vHcAsbciRmIVf0LiHLkMywfjQZUkceL+cxQdJU47YD1sNgmVKp6HxrIMunrs5mXc56WpxVtcEqEz6s1YWOeaTQd1qh/4nl8i0J/yUJ3T2TsOxRO7NQk6Ukxt5JqT3dAsd7VSYwHqd4sHXqrDb8ezadtdh0Ay0y7cbjVdPtmcjUPYxL1ovnktvi0pBEiWG7AsPgjSSuWL5KRMYYkgXdxYYzaNeuEKJRlu5uFw2McwYNSMRFHq9rKqZUcZ9saCpEcIb7XvgIN5WbyYNb8pih5FbS/ZKsWH+P2Gy6zWazkuvdxtzQbXrH6jbdFqa3iF4BnW2KJb4n1vEdew2E8+du+b19ivSrLsFP3vHLarq75Vt7lmnNHvO9g58GgV7+FHC+10z5NgYch2H2qQ0C5QlrfPIyMWpPAqNMxstBADL1NglsmwTsTU3oSSeBshDWQP1GE8IRTHN4inPA+i//jvPcc0BZKZoQiDymtllk2IlhsD6LOdbxMECx+m+2KIXVH9zO5R8=</diagram></mxfile>'
  }
  const file = new LocalFile(this, mockFile.data, mockFile.title, this.mode);
  this.loadFile(`-1`, true, file);
}
else if (!mxClient.IS_CHROMEAPP && (this.mode == null || force)) {
  ...
}
```

上面代码进入页面的时候会判断链接上是否携带参数 `id`，如果携带那么就会构造一个简单的 DrawIO 文件数据对象，之后再通过 `loadFile` 这个方法把文件绘制到画布上，具体效果如下图所示：

![img](https://pic4.zhimg.com/v2-acb581dc16c05fa083274e7dc0c29bdf_b.jpg)

基于此案例基本上就能把整个 DrawIO 二开的核心技术细节讲解完毕了，整个二开流程也可以很清晰的串联起来了，相信各位脑海中已经有了自己的宏伟蓝图，那么就可以把想法付诸于实际了。

>  如果想实际访问一下体验一下效果，笔者这边简单部署了一个个人版 DrawIO，大家可以通过如下链接进行访问：

- [个人版 DrawIO](https://link.zhihu.com/?target=https%3A//simple-drawio-starter.vercel.app/)
- [DrawIO 文件下载](https://link.zhihu.com/?target=https%3A//simple-drawio-starter.vercel.app/drawio/filesave/index.html)
- [DrawIO loadFile 加载文件](https://link.zhihu.com/?target=https%3A//simple-drawio-starter.vercel.app/drawio%3Fid%3D26)

## **DrawIO 二开 —— 核心架构**

前面讲了很多开发流程并附带踩坑细节，那么笔者除了帮大家踩坑之外，针对近期的 DrawIO 二开经历也进行了一些总结并且设计了一个 DrawIO 二开的开发架构，下面就简单介绍一下这个架构的设计过程。

其实 DrawIO 二开更多的是开发一个以绘图功能为核心的闭环系统，因此除了基本的绘图功能外，如何巧妙的把 DrawIO 绘图功能整合到系统里才是最为关键的部分，对于此我总结了三种架构模式：

### **架构一 —— 单一静态站模式**

![img](https://pic1.zhimg.com/v2-96d572200142ce0de066709ca0794020_b.jpg)

**单一静态站模式**整体架构如上图所示，整个架构就是以 DrawIO 静态站目录 `src/main/webapp` 为根目录，按照最原始的静态站开发模式进行开发。其中入口文件是 `index.html`，加载绘图相关的核心依赖 `js` 文件，完成绘制功能，其他功能页面和绘图页面也没什么耦合逻辑，就可以在工程目录里新建几个 `html` 文件，一个 `html` 文件对应一个功能，来进行项目的开发迭代。比如，首页就是 `home.html + home.js`， 列表页就是 `list.html + list.js` 等，所有开发都回归到最原始前端开发模式 — `HTML + JavaScript + CSS`。

> **优点：** 逻辑简单易懂，基于 DrawIO 源码仓库目录进行增量开发，易上手
>  **缺点：** 原始的开发模式带来的就是效率低下，原生开发模式没有使用流行的框架和打包工具，不太符合现代的开发模式。

### **架构二 —— 双系统模式**

![img](https://pic2.zhimg.com/v2-12452f29465e5e69f72a3d105217728d_b.jpg)

基于第一种静态站架构模式分析可以得知，该种模式其实不是很符合现今前端的开发节奏，原始的方式效率低下是最为致命的，那么改造后就形成了第二种架构模式 —— 双系统模式。此模式下有两个系统：DrawIO 绘图系统和兄弟业务系统，其实在整个系统里，核心的绘图功能和其他的业务需求是可以完全解藕的，那么就可以把与绘图无关的业务抽离出来单独形成一个系统，并且这个系统可以采用现代的开发模式进行开发，并且兄弟系统的架构选型也没有任何限制，想用什么开发就用什么开发。

> **优点：** 双系统相互独立互不影响，兄弟业务系统还可以用最新的框架和技术，提升效率。
>  **缺点：** 开发迭代需要维护两个系统，两个域名两套环境增加维护成本。

### **架构三 —— 特殊单系统模式**

由上面两种架构分析得知，架构一因为原始的开发模式导致开发复杂度上升，架构二通过解藕形成双系统提升开发效率但是引入了维护双系统的额外成本导致管理复杂度上升。因此把两个架构的优缺点进行整合，最后形成了第三个架构 —— **特殊单系统模式**。这种架构结合了上面两个架构的优点，并且通过一些特别的方案提升了 开发效率，架构图如下图所示：

![img](https://pic4.zhimg.com/v2-304a2db2de309df5e4d912f68aa74c57_b.jpg)

**下面着重讲解一下这个特殊单系统架构是如何解决上面提到的开发和管理复杂度问题的。**

解决开发复杂度其实很简单，引入一个合适的现代化框架就可以了，在这里我引入的是 Next.js，为什么引入 Next.js 而不是其他的现代化框架呢？原因有如下三点：

- 首先，它是基于 React.js 的 SSR 框架，因此符合现代框架提升开发效率的要求。
- 其次，Next.js 自带很多特性可以帮助解决管理复杂度，后面会讲到。
- 最后，笔者个人非常喜欢 Next.js，这个理由是最充分的。

下面是整个架构的核心 `package.json` 文件的部分代码，其实没很多天花乱坠的复杂操作，就是通过几个命令帮助开发者把绘图系统和业务系统桥接起来形成一个外部看起来相对统一的系统，因为是基于现代模式桥接而成的特殊单系统，因此开发部署就都非常的容易了。

```text
{
  "name": "overmind-x-graph",
  "version": "0.1.0",
  "author": "luffyZh <zhoudeyou@126.com>",
  "keywords": ["drawio", "react", "next.js"],
  "scripts": {
    "start": "next dev",
    "build": "next build",
    "prod": "next start",
    "drawio-dev": "cd drawio-project/src/main/webapp && serve",
    "drawio-build": "source ~/.bash_profile && cd drawio-project/etc/build && ant",
    "gen-drawio": "rm -rf public/drawio && cp -r drawio-project/src/main/webapp public/drawio",
    "analyze": "ANALYZE=true next build",
    ...
  },
  ...
}
```

### **脚本命令说明**

- `yarn drawio-dev` —— 开发模式调试 DrawIO 绘图系统

这个命令就是帮助开发者进入绘图系统的开发模式，虽说把两个系统桥接到一起，但是在开发的时候还是建议把绘图系统当成一个独立系统去看待，并且在 DrawIO 二开过程中涉及的改动绘图源码的部分都是在绘图系统里完成的，所以 DrawIO 绘图系统的开发模式非常重要。

- `yarn drawio-build` —— 编译 DrawIO 系统

在 DrawIO 二开完毕或者阶段性测试部署的时候，需要把开发环境修改的文件构建成生产环境的版本，此时使用此命令即可完成构建工作。

- `yarn gen-drawio` —— 同步最新绘图系统到主系统

如何桥接绘图系统与业务系统？就是通过这个命令来实现，绘图系统被当做一个单独的子系统进行开发和编译，编译完成后只需要执行此命令把主系统所需的绘图系统核心文件拿过来即可。

- `next.config.js` —— 主系统配置文件，桥接子系统

```text
const fs = require('fs');
const path = require('path');

...

module.exports = {
  reactStrictMode: true,
  // 重写 DrawIO 静态站地址，桥接主子系统
  rewrites: async () => drawioPaths,
};
```

那么主系统是通过何种方式完成绘图系统与业务系统的桥接呢？答案就是通过 Next.js 的 [Rewrites](https://link.zhihu.com/?target=https%3A//nextjs.org/docs/api-reference/next.config.js/rewrites) 特性，感兴趣的可以去查阅官方文档，正是因为这个特性才使得单系统进行 DrawIO 二开变成了可能并且开发起来也非常便利，**桥接双系统让整个工程对外形成一个独立完整系统**算是这个架构的核心之处。

这里多说一句，可能大家乍看之下不会感觉到啥，但是这个架构的最终敲定确实是笔者经过不断的开发尝试才敲定下来的，感兴趣的可以去跑一下代码去实际体验一下。另外选择一个适合你自己的框架就行了，不一定非得是 Next.js，如果你是基于 Vue 的开发者，那么 Nuxt 是不是就在那里静静地等着你呢？本文只是提供一个思路，各位可以发散思路，毕竟自己探索的过程才更有意思。

>  讲这么多其实都帮各位看官准备好了，基于**架构三**搭建了一个脚手架 [simple-drawio-starter](https://link.zhihu.com/?target=https%3A//github.com/luffyZh/simple-drawio-starter)，大家直接拿来进行业务逻辑的开发即可。
>  

## **总结**

DrawIO 功能强大的同时整个源码的文件体系结构也十分庞大复杂，因此读起来还是颇费些功夫，从确立需求到阅读源码再到最后二开总结架构，一不小心就写成了万字长文，希望本文能对那些有类似需求或者想要做一个类似工具的朋友提供一些帮助，其实 DrawIO 的开发难点也就是在于相关的技术文档比较少，社区大部分都是使用教程而不是二开教程，难以找到一个好的入门开发文档算是笔者写这篇文章的一个初衷，也是希望大家能少走些弯路。

最后，非常感谢各位耐心地阅读完本篇文章，如果在 DrawIO 开发过程有更好的见解和思路欢迎留言交流，不胜感激～







## 参考文档

[drawio 官方文档](https://link.zhihu.com/?target=https%3A//www.diagrams.net/doc/)

[drawio 二次开发优秀项目](https://link.zhihu.com/?target=http%3A//processflow.sharehoo.cn/)

[drawio 编码和解密工具](https://link.zhihu.com/?target=https%3A//jgraph.github.io/drawio-tools/tools/convert.html)

[drawio 使用教程](https://zhuanlan.zhihu.com/p/129659662)

[drawio 画流程图](https://link.zhihu.com/?target=https%3A//blog.csdn.net/zyx_ly/article/details/102488457)



drawIo二次开发





既然是基于 DrawIO 进行二开，首先就来了解一下 DrawIO 到底是什么，跟着笔者视角先去 [DrawIO 官网](https://link.zhihu.com/?target=https%3A//www.diagrams.net/)去看一圈，官网首页截图如下所示：

![img](https://pic4.zhimg.com/v2-94d26321e58a22ccf005540afc2d618b_b.jpg)

简要来说**DrawIO 是一个支持绘制各种形式图表并且支持多位置存储能力的网站和工具**。绘制图表的功能就不多说了，这也是 DrawIO 自身的核心功能，这里着重介绍一下 DrawIO 的多位置存储能力，通过 DrawIO 绘制的图表可以存在网络上的很多地方，比如：浏览器缓存、本机文件夹、远程存储（Github、Gitlab 和 Google Drive 等）。存储浏览器和本机文件夹很简单不做介绍，这里以 Github 为例简单讲解一下通过 DrawIO 绘制的图表如何进行远程存储。

### **存储 DrawIO 到 Github 仓库**

- 1 - 在 Github 新建一个仓库

这里笔者新建了一个名为 [drawio-folder](https://link.zhihu.com/?target=https%3A//github.com/luffyZh/drawio-folder) 的仓库，里面只有一个 `README.md` 文件，如下图所示：

![img](https://pic4.zhimg.com/v2-849ecc2b112431553c97e7dc4d8b03f3_b.jpg)

- 2 - 绘图时选择 GitHub 作为存储空间

然后从官网点击 Start 按钮进入绘图页面，第一次进入页面不出意外会出现一个如下图所示选择存储空间的选项，然后选择 Github（其他远程存储原理一样）。

![img](https://pic1.zhimg.com/v2-e1c7e7893a4102741e76b5540ce5394c_b.jpg)

- 3 - 选择对应的仓库然后绘制图片

点击选择存储到 Github 仓库，然后选择新建一个图表，命名之后点击新建 Create 按钮完成图表的创建。

![img](https://pic3.zhimg.com/v2-a0e09c81909fe85174dc656dfe80964a_b.jpg)

- 4 - 授权选取存储仓库

图标创建成功后，会出现一个授权登录的弹窗，授权登录过后 DrawIO 就能获取到账户下的所有仓库信息，选择刚才创建的 **drawio-folder** 仓库，然后进行图表的绘制。

![img](https://pic3.zhimg.com/v2-45e8cc7bcffaeca717c3b819ee9a7d4a_b.jpg)

- 5 - 绘图完毕保存文档

绘制过程中，每一次的保存操作，都会创建一条 `commit message`，就相当于正常的仓库修改并提交，如下图所示：

![img](https://pic2.zhimg.com/v2-1785a17906c1680f2616e7fa7ac0d301_b.jpg)

当一次图表绘制完成并保存后，可以去 Github 仓库看看是否存储成功。

![img](https://pic1.zhimg.com/v2-6e70a39a83b52df44114094591c733ac_b.jpg)

可以看到，刚才创建的图表已经成功存储到了 Github 仓库，非常的便捷，基本上就等于你已经有了一个无限制的图表云盘一样。

至此相信大家应该已经了解了 DrawIO 如何绘图以及它强大的存储功能，在文章的开头提到过，如果你是一个单纯的使用者，那么文章读到这里已经足够了，你已经拥有了一个免费且强大的绘图工具（不需要开通会员不需要注册也没有绘制数限制）以及很多类型的无限存储空间，相比还要收费的 Some Apps 它不香吗？

### **为什么要进行 DrawIO 二开**

既然 DrawIO 本身已经这么香了，为啥还要进行二开呢？在这简单总结几点为什么要进行二开或者可能需要进行二开的原因：

- 首先，对于一个团队来说需要的不仅仅是一个纯绘图工具，而是一个包含绘图功能的一体化管理平台，绘图只是其中最核心的部分，其余还有一些必不可少的功能。
- 其次，在一些特殊业务需求中，需要把图表和自身的业务结合起来以发挥图表更大的能力或者基于业务进行特殊改造，这种场景的话二开就是必然的。
- 最后，就是所谓的逼格问题，直白点说就是能放到自己公司独立部署服务的，为啥放在外面？免费版万一有一天不免费了或者有限制了那就被动了。（虽然可能性极低，但是也要考虑这种因素）

>  如果你或者你的团队也有上面几点原因，那么欢迎继续往下看，接下来会介绍如何以 DrawIO 官方开源代码为基础进行简单的二次开发。

## **DrawIO 二开 —— 流程准备**

> **【注意】**：本文主要针对前端开发者，但是在开发部署构建新版本的过程有很多依赖 Java 环境的内容，所以为了保证文章的连贯性，也会进行简单介绍，如果你已经熟悉相关环境配置，那么忽略对应内容即可。

### **环境准备**

- 前端开发环境

这里就不多说啥了，其实 DrawIO 的开发还真就是完全属于前端范畴，所以各位前端同学如果遇到类似的需求，就迎难而上吧～

- 编译构建相关的环境 —— Java 和 Ant

>  关于 Java 和 Ant 两个环境的安装，这边以 MacOS 为例进行讲解，其他系统参考官方教程。

至于为啥要用到这俩，后面构建部署过程会介绍到，因为前端开发同学的电脑里大概率不会安装这俩，所以这里也简单讲一下安装步骤。

```text
# jdk 下载地址
https://www.oracle.com/java/technologies/javase-jdk16-downloads.htmlt

# ant 下载地址
https://ant.apache.org/bindownload.cgi
```

对于 jdk 安装包下载完之后正常安装即可，检验是否安装成功可以使用 `javac` 或者 `java -version` 命令来查看安装版本，之后将下载的 Ant 压缩包解压到 Applications 目录下，然后运行如下命令。

```text
# 写入环境变量
sudo vim .bash_profile
# 这里注意，版本号和你自己下载的要对应上
export ANT_HOME=/Applications/apache-ant-1.9.16
export PATH=$ANT_HOME/bin:$PATH
# 退出并保存
:wq
# 让环境变量生效
source .bash_profile
# 查看版本
ant -V
```

![img](https://pic2.zhimg.com/v2-6028281e3ddc71bcdd6da747b5566105_b.jpg)

安装完成之后可以在命令行验证是否安装成功，如果命令行里出现了如上图所示对应的 Java 和 Ant 版本信息，那么恭喜你 DrawIO 二开的基础环境已经准备完成，接下来就是正式进入 DrawIO 二开了。

### **DrawIO 源码**

二开的开始，首先从 [DrawIO 源码仓库](https://link.zhihu.com/?target=https%3A//github.com/jgraph/drawio) Clone 官方代码，在本地跑一下看看效果。老规矩 Clone 下来先阅读 `package.json` 文件，发现其中只有一个 `start` 命令。

```text
"scripts": {
  "start": "electron ."
}
```

了解前端的同学应该能知道，此命令将会启动了一个 Electron 桌面应用，运行此命令过后不出意外的，一个桌面版 DrawIO 就呈现在眼前了。

![img](https://pic1.zhimg.com/v2-0c6730b61633d20c0ca4d681e38e0170_b.jpg)

它的使用方式和上面文章里介绍的一样，但是问题来了！我们最终目标是要进行 DrawIO 二开，难不成是基于 Electron 二开吗？虽然说也不是不行，但是那上手成本可就高了，估计会劝退一大波同学，不要慌，去官方仓库看看有没有其他方法。

![img](https://pic4.zhimg.com/v2-4b75206f4131c8dbdc3c757cebef2a2b_b.jpg)

功夫不负苦心人，如上图官方文档介绍了除了基于 Electron 部署应用外，DrawIO 还能以静态站的形式部署到 Github Pages，并且官方的站点地址点开后就更清晰了：`https://jgraph.github.io/drawio/src/main/webapp/index.html`。从链接可以观察到其实 Github Pages 部署的站点就是 `/src/main/webapp` 这个文件夹，也就是 DrawIO 的静态站根目录，那么静态站的开发对于前端来说再熟悉不过了，所以接下来所有的二开过程都是基于静态站的形式进行讲解。

>  如果大家目标就是基于 Electron 构建桌面端应用，也可以对应的进行相应的二开，流程上和静态站的二开基本一致。开发模式的选型主要是通过分析自己团队的业务诉求得出结论，因为网页类应用普适性比较好并且没有那么多局限性，所以云音乐选择的就是静态站的模式。

### **开发模式及预览**

一个陌生的应用系统或者框架怎么熟悉最快，最简单的办法就是顺着入口文件一点一点的啃下去了，这与平时阅读源码的过程很类似，DrawIO 静态站的入口文件是 `/src/main/webapp/index.html`，接下来从它入手，简单介绍一下如何在本地开发调试 DrawIO 代码。

- 第一步：新建 `package.json`

```text
{
  "name": "drawio-dev",
  "version": "1.0.0",
  "main": "src/main/webapp/index.html",
  "scripts": {
    "start": "cd src/main/webapp && serve"
  },
  "license": "MIT",
  "dependencies": {
    "serve": "^12.0.0"
  }
}
```

首先，根目录新建一个 `package.json` 文件，然后键入如上内容，然后通过 `yarn start` 或者 `npm run start` 命令，就能很简单的在本地启动一个服务器，进行开发访问，效果如下：

![img](https://pic1.zhimg.com/v2-f249b749385314f8a3a2433152fb20ec_b.jpg)

- 第二步：修改代码，开发模式预览

由于 DrawIO 项目的文件体系十分的庞大，这里不做过多介绍，主要给大家介绍一下二开过程中核心的一个文件目录 `src/main/webapp/js/diagramly`，此文件夹内的文件是系统运行最为核心的部分，二开的工作大部分也都是修改此文件夹的文件内容。

其中，`src/main/webapp/js/diagramly/App.js` 算是加载 DrawIO 的一个入口 js 文件，我们在其中第一行键入一段代码：

![img](https://pic1.zhimg.com/v2-3d92062086e647e4fc53efbc524b3784_b.jpg)

很容易理解，就是 Alert 一个弹窗提示测试一下代码是不是能正常执行，不过刷新页面之后会发现并没有看到这个弹窗，这是为什么呢？打开控制台找找原因：

![img](https://pic4.zhimg.com/v2-29b79f0979a6d0116a9103eb49268903_b.jpg)

发现加载的不是 `app.js` 而是 `app.min.js`，看来默认加载的是生产版本的代码，那么如何执行开发代码进入开发模式呢？仔细阅读源码会发现，DrawIO 是通过识别 url query 的 `dev` 参数来判断是不是开发模式（`dev === 1` 则为开发模式），那么加上这个参数再来试试：

![img](https://pic2.zhimg.com/v2-3ada001f9b1fc6006aa381c3eb592e01_b.jpg)

到这里，笔者内心莫名出现了几万头不知名的可爱动物，果然没有这么容易啊。没事，技术就是一个不断探索的过程，希望各位看官能和我一样不要放弃。

> **当然，看完本篇文章你们就已经少走了 90% 的弯路，直接进行二开就可以了。**

为啥会找不到文件呢？简单分析一下，还是文件的指向问题：

![img](https://pic4.zhimg.com/v2-ca51bcaaf92984005a81b35d9f634987_b.jpg)

在本地开发应该请求的是本地的资源文件，但是 Network 面板发现请求的资源地址并不对，废话不多说，再说下去可能大家内心也崩溃了，通过如下代码，把开发模式安排明白。

```text
// index.html 从 245 行开始

// Changes paths for local development environment
if (urlParams['dev'] == '1') {
    ...

  // ====> 开发模式文件指向本地
  if (location.hostname == 'localhost' || location.hostname == '127.0.0.1') {
    drawDevUrl = `http://${location.host}/`;
    geBasePath = `http://${location.host}/js/grapheditor`;
    mxBasePath = `http://${location.host}/mxgraph`;
    mxForceIncludes = true;
  }
  // ====> 开发模式文件指向本地

  mxscript(drawDevUrl + 'js/PreConfig.js');
  mxscript(drawDevUrl + 'js/diagramly/Init.js');
  mxscript(geBasePath + '/Init.js');
  mxscript(mxBasePath + '/mxClient.js');
  ...
}
```

核心代码以及对应位置就在上面已经给出了，修改过后直接刷新页面看一下效果：

![img](https://pic2.zhimg.com/v2-553c6a9e7adaf591f782850a7a88e7d9_b.jpg)

可以看到，我们心心念念的 Alert 弹窗已经出来了，此时此刻，笔者已经是老泪纵横了，到这里为止，DrawIO 二开的开发流程基本介绍完成，关于二开的更多细节如何去改造 DrawIO 就是各位根据自身团队的需求而定了。

### **构建生产版本**

上面搞定了开发模式，完成了本地开发实时预览，但是最终发布的代码肯定还是要用生产版本，前面也知道了，生产版本使用的是诸如 `app.min.js` 这种压缩文件，那么如何把本地开发的代码打包编译为生产版本呢？这就要用到前面已经安装好的 Java 和 Ant 环境了。

这里没有过多内容，非常简单的在 `package.json` 文件里增加如下代码即可：

```text
{
  "name": "drawio-dev",
  "version": "1.0.0",
  "main": "src/main/webapp/index.html",
  "scripts": {
    "start": "cd src/main/webapp && serve",
+   "build": "cd etc/build && ant"
  },
  "license": "MIT",
  "dependencies": {
    "serve": "^12.0.0"
  }
}
```

增加完上述 `build` 命令之后运行一下，不出意外会出现如下构建成功的提示：

![img](https://pic3.zhimg.com/v2-83e8347b33ecfd1dc1f631407f81316a_b.jpg)

然后再来访问一下首页，这次不携带 `dev=1` 开发参数看看效果，如下图所示，非常完美的运行着新开发的生产版本，至此为止 DrawIO 的二开流程阶段，从开发预览到构件生产版本都已经介绍完成。

![img](https://pic3.zhimg.com/v2-4a0933e4c57f73cdb76661952530086a_b.jpg)

### **部署**

部署的话，其实就参考静态站部署就可以了，核心目录就是上面提到过的 `src/main/webapp`。笔者在这里简单给各位提供几个部署思路：

- Github Pages(个人)
- Vercel(个人)
- Nginx(团队)
- Node Server(团队)

至于如何部署，各位根据自身或者团队实际情况进行选择即可。

## **DrawIO 二开 —— 技术细节**

笔者基于 DrawIO 开发的绘图管理系统在云音乐团队内部内测中，目前来说已完成的和正在开发中的功能集如下：

*√*基础图绘制功能

*√*基于云音乐内部服务的增删改查

*√*个人中心模块，管理自己的图表内容

*√*数据交互，图表与业务系统进行数据交互，一键导入已创建的图表

*x*Todo - 鉴权功能，比如分享，指定权限的人可以进行相应操作 

*x*Todo - 团队空间，一个团队可以管理一个图表的集合

*x*Todo - 版本历史，支持查看最近几次的版本修改内容

*x*Todo - 自定义导入一些基于公司/个人比较常用的模板

由于本文还是偏技术向的文章，上面很多功能其实和 DrawIO 二开没啥太大的关系，都是一些基于业务的需求，所以不做过多细节介绍，笔者相信各位开发者/团队进行 DrawIO 二开的主要原因，80% 以上都是因为想要把内容保存到内部服务上，也就是说获取当前画板并按照一定的形式保存在数据库里，这才是整个二开过程中最为核心的部分。这里以一个二开过程中的源码案例扩展讲一下 DrawIO 二开过程中的部分技术细节。

### **获取当前画板内容 —— `getCurrentFile`**

那么如何获取当前画板内容呢？还是通过对 `App.js` 源码的阅读，我发现了如下这段代码：

```text
App.prototype.save = function(name, done) {
    var file = this.getCurrentFile();
  ...
}
```

顾名思义，这段代码会在保存的时候执行，并且会获取到一个 file 文件对象，添加一个 log 后运行一下代码，看看效果：

![img](https://pic4.zhimg.com/v2-1276bbd29759a59013c77d0f6b187507_b.jpg)

可以看到，确实打印出来了一个 file 文件对象，关于对象里面的众多属性这里不一一说明，只介绍一个最核心的 `data` 属性也就是文件数据，有了这个文件数据能做什么呢？别着急，接着往下看。

### **构造文件下载到本地**

通过 `getCurrentFile` 我们从画布获取到了当前正在绘制的图表数据，看内容是一段 xml 字符串，接下来就通过这个字符串反向构造一个文件下载下来，看看是不是和画布上的那个图表一模一样。此处通过 `filesaver` 将构造的文件数据进行下载，核心代码如下：

```text
/**
 * 构造文件下载到浏览器
 **/
function download() {
  const fileObj = {
    title: 'luffyzh.drawio',
    data: '<mxfile host="localhost" modified="2021-09-02T09:50:39.574Z" agent="5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36" etag="PyBU88KGLdBE5FU7fpPf" version="@DRAWIO-VERSION@" type="device"><diagram id="C5RBs43oDa-KdzZeNtuy" name="Page-1">7VhZc5swEP41PLbD4WDnsThx0tbNpE3aTB9lkIVqwVIhfOTXdzHCgPH4aOscnvjBo12WlbTft4dtOP1ofiVJEn6BgArDNoO54VwYtn3WNfE7VywKhduzCwWTPChUVqW4449UK/V7LOMBTRuGCkAonjSVPsQx9VVDR6SEWdNsDKK5a0IYbSnufCLa2gceqLDQ9uxupb+mnIXlzpZ7XjyJSGmsb5KGJIBZTeVcGk5fAqhiFc37VOSxK+Py8HHxIIYT9+rT1/Q3+e59vr/58a5wNjjkldUVJI3V/3WtsZwSkel46buqRRlACVkc0NyJaTheqCKBSwuXv6hSCw04yRSgCqQKgUFMxBAg0XZjiJU2s3KZxsGHHFiURwL8SaEacCH0Hihp+x5KqZIwWWGXO1gBkRsLMqLCI/6ELQ/aBwESH8UQ09xVgGTQd6kOd1lpvT1jqzFIIZM+3WLnaIoTyeg2f25hl5+vxlON3BWFiCq5QANJBVF82iQz0TnBVnYV7rjQ0B9AA6dFA5GNx4vHcAsbciRmIVf0LiHLkMywfjQZUkceL+cxQdJU47YD1sNgmVKp6HxrIMunrs5mXc56WpxVtcEqEz6s1YWOeaTQd1qh/4nl8i0J/yUJ3T2TsOxRO7NQk6Ukxt5JqT3dAsd7VSYwHqd4sHXqrDb8ezadtdh0Ay0y7cbjVdPtmcjUPYxL1ovnktvi0pBEiWG7AsPgjSSuWL5KRMYYkgXdxYYzaNeuEKJRlu5uFw2McwYNSMRFHq9rKqZUcZ9saCpEcIb7XvgIN5WbyYNb8pih5FbS/ZKsWH+P2Gy6zWazkuvdxtzQbXrH6jbdFqa3iF4BnW2KJb4n1vEdew2E8+du+b19ivSrLsFP3vHLarq75Vt7lmnNHvO9g58GgV7+FHC+10z5NgYch2H2qQ0C5QlrfPIyMWpPAqNMxstBADL1NglsmwTsTU3oSSeBshDWQP1GE8IRTHN4inPA+i//jvPcc0BZKZoQiDymtllk2IlhsD6LOdbxMECx+m+2KIXVH9zO5R8=</diagram></mxfile>'
  }
  const blob = new Blob([fileObj.data], {type: 'application/octet-stream'});
  saveAs(blob, fileObj.title);
}
```

从代码和截图效果可以得出结论：通过 `getCurrentFile` 获取到图表核心数据 data，就能很容易的反向构造出来一个 .drawio 文件也就是绘图文件，并且和画布上的文件一模一样（感觉自己好像在说废话，当然一模一样了，数据都是一样的他不一样可就奇怪了）。

### **将保存的图表数据绘制到画布上 —— `loadFile`**

上面的两步我们确定了画布数据的获取以及数据是否能反向构造出来图表文件，由此确定了技术方案的可行性，但是其实还差最后一个环节 —— 加载图表数据到画布上，因为你的图表不仅仅能保存，还需要能继续编辑，因此把数据从文件里获取出来然后反向构造成图表数据并加载到画布上，这才完成了一个 DrawIO 二开的最小闭环。这里不做过多废话，核心 API — `loadFile`，细节代码如下：

```text
if (this.editor.isChromelessView()) {
    ...
}
/**
 * 如果有id，表示打开了一个存在的文档，进入编辑模式
 */
else if (urlParams['id']) {
  const mockFile = {
    title: 'luffyzh.drawio',
    data: '<mxfile host="localhost" modified="2021-09-02T09:50:39.574Z" agent="5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36" etag="PyBU88KGLdBE5FU7fpPf" version="@DRAWIO-VERSION@" type="device"><diagram id="C5RBs43oDa-KdzZeNtuy" name="Page-1">7VhZc5swEP41PLbD4WDnsThx0tbNpE3aTB9lkIVqwVIhfOTXdzHCgPH4aOscnvjBo12WlbTft4dtOP1ofiVJEn6BgArDNoO54VwYtn3WNfE7VywKhduzCwWTPChUVqW4449UK/V7LOMBTRuGCkAonjSVPsQx9VVDR6SEWdNsDKK5a0IYbSnufCLa2gceqLDQ9uxupb+mnIXlzpZ7XjyJSGmsb5KGJIBZTeVcGk5fAqhiFc37VOSxK+Py8HHxIIYT9+rT1/Q3+e59vr/58a5wNjjkldUVJI3V/3WtsZwSkel46buqRRlACVkc0NyJaTheqCKBSwuXv6hSCw04yRSgCqQKgUFMxBAg0XZjiJU2s3KZxsGHHFiURwL8SaEacCH0Hihp+x5KqZIwWWGXO1gBkRsLMqLCI/6ELQ/aBwESH8UQ09xVgGTQd6kOd1lpvT1jqzFIIZM+3WLnaIoTyeg2f25hl5+vxlON3BWFiCq5QANJBVF82iQz0TnBVnYV7rjQ0B9AA6dFA5GNx4vHcAsbciRmIVf0LiHLkMywfjQZUkceL+cxQdJU47YD1sNgmVKp6HxrIMunrs5mXc56WpxVtcEqEz6s1YWOeaTQd1qh/4nl8i0J/yUJ3T2TsOxRO7NQk6Ukxt5JqT3dAsd7VSYwHqd4sHXqrDb8ezadtdh0Ay0y7cbjVdPtmcjUPYxL1ovnktvi0pBEiWG7AsPgjSSuWL5KRMYYkgXdxYYzaNeuEKJRlu5uFw2McwYNSMRFHq9rKqZUcZ9saCpEcIb7XvgIN5WbyYNb8pih5FbS/ZKsWH+P2Gy6zWazkuvdxtzQbXrH6jbdFqa3iF4BnW2KJb4n1vEdew2E8+du+b19ivSrLsFP3vHLarq75Vt7lmnNHvO9g58GgV7+FHC+10z5NgYch2H2qQ0C5QlrfPIyMWpPAqNMxstBADL1NglsmwTsTU3oSSeBshDWQP1GE8IRTHN4inPA+i//jvPcc0BZKZoQiDymtllk2IlhsD6LOdbxMECx+m+2KIXVH9zO5R8=</diagram></mxfile>'
  }
  const file = new LocalFile(this, mockFile.data, mockFile.title, this.mode);
  this.loadFile(`-1`, true, file);
}
else if (!mxClient.IS_CHROMEAPP && (this.mode == null || force)) {
  ...
}
```

上面代码进入页面的时候会判断链接上是否携带参数 `id`，如果携带那么就会构造一个简单的 DrawIO 文件数据对象，之后再通过 `loadFile` 这个方法把文件绘制到画布上，具体效果如下图所示：

![img](https://pic4.zhimg.com/v2-acb581dc16c05fa083274e7dc0c29bdf_b.jpg)

基于此案例基本上就能把整个 DrawIO 二开的核心技术细节讲解完毕了，整个二开流程也可以很清晰的串联起来了，相信各位脑海中已经有了自己的宏伟蓝图，那么就可以把想法付诸于实际了。

>  如果想实际访问一下体验一下效果，笔者这边简单部署了一个个人版 DrawIO，大家可以通过如下链接进行访问：

- [个人版 DrawIO](https://link.zhihu.com/?target=https%3A//simple-drawio-starter.vercel.app/)
- [DrawIO 文件下载](https://link.zhihu.com/?target=https%3A//simple-drawio-starter.vercel.app/drawio/filesave/index.html)
- [DrawIO loadFile 加载文件](https://link.zhihu.com/?target=https%3A//simple-drawio-starter.vercel.app/drawio%3Fid%3D26)

## **DrawIO 二开 —— 核心架构**

前面讲了很多开发流程并附带踩坑细节，那么笔者除了帮大家踩坑之外，针对近期的 DrawIO 二开经历也进行了一些总结并且设计了一个 DrawIO 二开的开发架构，下面就简单介绍一下这个架构的设计过程。

其实 DrawIO 二开更多的是开发一个以绘图功能为核心的闭环系统，因此除了基本的绘图功能外，如何巧妙的把 DrawIO 绘图功能整合到系统里才是最为关键的部分，对于此我总结了三种架构模式：

### **架构一 —— 单一静态站模式**

![img](https://pic1.zhimg.com/v2-96d572200142ce0de066709ca0794020_b.jpg)

**单一静态站模式**整体架构如上图所示，整个架构就是以 DrawIO 静态站目录 `src/main/webapp` 为根目录，按照最原始的静态站开发模式进行开发。其中入口文件是 `index.html`，加载绘图相关的核心依赖 `js` 文件，完成绘制功能，其他功能页面和绘图页面也没什么耦合逻辑，就可以在工程目录里新建几个 `html` 文件，一个 `html` 文件对应一个功能，来进行项目的开发迭代。比如，首页就是 `home.html + home.js`， 列表页就是 `list.html + list.js` 等，所有开发都回归到最原始前端开发模式 — `HTML + JavaScript + CSS`。

> **优点：** 逻辑简单易懂，基于 DrawIO 源码仓库目录进行增量开发，易上手
>  **缺点：** 原始的开发模式带来的就是效率低下，原生开发模式没有使用流行的框架和打包工具，不太符合现代的开发模式。

### **架构二 —— 双系统模式**

![img](https://pic2.zhimg.com/v2-12452f29465e5e69f72a3d105217728d_b.jpg)

基于第一种静态站架构模式分析可以得知，该种模式其实不是很符合现今前端的开发节奏，原始的方式效率低下是最为致命的，那么改造后就形成了第二种架构模式 —— 双系统模式。此模式下有两个系统：DrawIO 绘图系统和兄弟业务系统，其实在整个系统里，核心的绘图功能和其他的业务需求是可以完全解藕的，那么就可以把与绘图无关的业务抽离出来单独形成一个系统，并且这个系统可以采用现代的开发模式进行开发，并且兄弟系统的架构选型也没有任何限制，想用什么开发就用什么开发。

> **优点：** 双系统相互独立互不影响，兄弟业务系统还可以用最新的框架和技术，提升效率。
>  **缺点：** 开发迭代需要维护两个系统，两个域名两套环境增加维护成本。

### **架构三 —— 特殊单系统模式**

由上面两种架构分析得知，架构一因为原始的开发模式导致开发复杂度上升，架构二通过解藕形成双系统提升开发效率但是引入了维护双系统的额外成本导致管理复杂度上升。因此把两个架构的优缺点进行整合，最后形成了第三个架构 —— **特殊单系统模式**。这种架构结合了上面两个架构的优点，并且通过一些特别的方案提升了 开发效率，架构图如下图所示：

![img](https://pic4.zhimg.com/v2-304a2db2de309df5e4d912f68aa74c57_b.jpg)

**下面着重讲解一下这个特殊单系统架构是如何解决上面提到的开发和管理复杂度问题的。**

解决开发复杂度其实很简单，引入一个合适的现代化框架就可以了，在这里我引入的是 Next.js，为什么引入 Next.js 而不是其他的现代化框架呢？原因有如下三点：

- 首先，它是基于 React.js 的 SSR 框架，因此符合现代框架提升开发效率的要求。
- 其次，Next.js 自带很多特性可以帮助解决管理复杂度，后面会讲到。
- 最后，笔者个人非常喜欢 Next.js，这个理由是最充分的。

下面是整个架构的核心 `package.json` 文件的部分代码，其实没很多天花乱坠的复杂操作，就是通过几个命令帮助开发者把绘图系统和业务系统桥接起来形成一个外部看起来相对统一的系统，因为是基于现代模式桥接而成的特殊单系统，因此开发部署就都非常的容易了。

```text
{
  "name": "overmind-x-graph",
  "version": "0.1.0",
  "author": "luffyZh <zhoudeyou@126.com>",
  "keywords": ["drawio", "react", "next.js"],
  "scripts": {
    "start": "next dev",
    "build": "next build",
    "prod": "next start",
    "drawio-dev": "cd drawio-project/src/main/webapp && serve",
    "drawio-build": "source ~/.bash_profile && cd drawio-project/etc/build && ant",
    "gen-drawio": "rm -rf public/drawio && cp -r drawio-project/src/main/webapp public/drawio",
    "analyze": "ANALYZE=true next build",
    ...
  },
  ...
}
```

### **脚本命令说明**

- `yarn drawio-dev` —— 开发模式调试 DrawIO 绘图系统

这个命令就是帮助开发者进入绘图系统的开发模式，虽说把两个系统桥接到一起，但是在开发的时候还是建议把绘图系统当成一个独立系统去看待，并且在 DrawIO 二开过程中涉及的改动绘图源码的部分都是在绘图系统里完成的，所以 DrawIO 绘图系统的开发模式非常重要。

- `yarn drawio-build` —— 编译 DrawIO 系统

在 DrawIO 二开完毕或者阶段性测试部署的时候，需要把开发环境修改的文件构建成生产环境的版本，此时使用此命令即可完成构建工作。

- `yarn gen-drawio` —— 同步最新绘图系统到主系统

如何桥接绘图系统与业务系统？就是通过这个命令来实现，绘图系统被当做一个单独的子系统进行开发和编译，编译完成后只需要执行此命令把主系统所需的绘图系统核心文件拿过来即可。

- `next.config.js` —— 主系统配置文件，桥接子系统

```text
const fs = require('fs');
const path = require('path');

...

module.exports = {
  reactStrictMode: true,
  // 重写 DrawIO 静态站地址，桥接主子系统
  rewrites: async () => drawioPaths,
};
```

那么主系统是通过何种方式完成绘图系统与业务系统的桥接呢？答案就是通过 Next.js 的 [Rewrites](https://link.zhihu.com/?target=https%3A//nextjs.org/docs/api-reference/next.config.js/rewrites) 特性，感兴趣的可以去查阅官方文档，正是因为这个特性才使得单系统进行 DrawIO 二开变成了可能并且开发起来也非常便利，**桥接双系统让整个工程对外形成一个独立完整系统**算是这个架构的核心之处。

这里多说一句，可能大家乍看之下不会感觉到啥，但是这个架构的最终敲定确实是笔者经过不断的开发尝试才敲定下来的，感兴趣的可以去跑一下代码去实际体验一下。另外选择一个适合你自己的框架就行了，不一定非得是 Next.js，如果你是基于 Vue 的开发者，那么 Nuxt 是不是就在那里静静地等着你呢？本文只是提供一个思路，各位可以发散思路，毕竟自己探索的过程才更有意思。

>  讲这么多其实都帮各位看官准备好了，基于**架构三**搭建了一个脚手架 [simple-drawio-starter](https://link.zhihu.com/?target=https%3A//github.com/luffyZh/simple-drawio-starter)，大家直接拿来进行业务逻辑的开发即可。
>  

## **总结**

DrawIO 功能强大的同时整个源码的文件体系结构也十分庞大复杂，因此读起来还是颇费些功夫，从确立需求到阅读源码再到最后二开总结架构，一不小心就写成了万字长文，希望本文能对那些有类似需求或者想要做一个类似工具的朋友提供一些帮助，其实 DrawIO 的开发难点也就是在于相关的技术文档比较少，社区大部分都是使用教程而不是二开教程，难以找到一个好的入门开发文档算是笔者写这篇文章的一个初衷，也是希望大家能少走些弯路。

最后，非常感谢各位耐心地阅读完本篇文章，如果在 DrawIO 开发过程有更好的见解和思路欢迎留言交流，不胜感激～





