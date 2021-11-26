# drawIo二次开发

 作者：营销助手
链接：https://zhuanlan.zhihu.com/p/420757780
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



## [http://drawio.io](https://link.zhihu.com/?target=http%3A//drawio.io)二次开发教程

## 二次开发

### 基础资料

[导入ide](https://link.zhihu.com/?target=https%3A//blog.csdn.net/keylkeaf/article/details/106012044) 

[增加本地和oss存储](https://link.zhihu.com/?target=https%3A//gitee.com/zero123123/draw.io)
 [纯前端功能](https://link.zhihu.com/?target=https%3A//gitee.com/twobear/drawio-webapp)
 [各种修改位置介绍](https://link.zhihu.com/?target=https%3A//github.com/qdteng/drawdb) 

### 修改代码

准备环境 

```text
git clone  https://github.com/jgraph/drawio.git
cd drawid-dev\src\main\webapp  
python -m http.server 8081
```

项目默认是使用打包好的 app.min.js,如果需要修改代码，则需要在请求参数增加 dev=1  

同时需要修改 index.html  line:245 注释掉远程地址 ，否则测试环境下不会请求本地修改后的js

```text
if (urlParams['dev'] == '1')
{
    // Used to request grapheditor/mxgraph sources in dev mode
    var mxDevUrl = document.location.protocol ; //+ '//devhost.jgraph.com/drawio/src/main';

    // Used to request draw.io sources in dev mode
    var drawDevUrl = document.location.protocol ; //+ '//devhost.jgraph.com/drawio/src/main/webapp/';
```

一切就绪...

### 修改菜单

菜单相关设置在 js/diagramly/Menus.js
 如要删除File --> share...功能  

在js/diagramly/Menus.js  搜索 share... 如下，注释掉代码即可  

```text
/*
    this.editorUi.actions.addAction('share...', mxUtils.bind(this, function()
    {
        try
        {
            var file = editorUi.getCurrentFile();

            if (file != null)
            {
                file.share();
            }
        }
        catch (e)
        {
            editorUi.handleError(e);
        }
    }));
*/
```

修改功能代码也是一样，如将share... 按钮,修改为打开百度，对应代码如下即可   

```text
this.editorUi.actions.addAction('share...', mxUtils.bind(this, function()
{
    editorUi.openLink('https://www.baidu.com/',"_blank")

}));
```

可以在浏览器控制台查看各种变量信息  如 

```text
console.log(mxResources.get('url'))
```

### 修改生效

不能每次都使用dev模式执行，因此需要对 修改后的文件进行 打包  

```text
cd drawio/etc/build
ant
```

### 具体功能实现

### 默认弹出SQL转ER框

发现 菜单的

```text
调整图形 -> 插入 -> 高级 -> SQL...
```

这个功能很有意思，可以直接将SQL语句转成ER图   

但是这个路径太长了，需要选择好几级菜单才可以找到这个功能    

怎样打开页面默认弹出这个编辑框?   

- 首先找到原始功能   

```text
调整图形 -> 插入 -> 高级 -> SQL...
```

的具体实现在   \src\main\webapp\js\diagramly\Menus.js 功能代码  

```text
if (method == 'fromText' || method == 'formatSql' ||
                        method == 'plantUml' || method == 'mermaid')
{
    var dlg = new ParseDialog(editorUi, title, method);
    editorUi.showDialog(dlg.container, 620, 420, true, false);
    editorUi.dialog.container.style.overflow = 'auto';
    dlg.init();
}
```

ParseDialog 具体实现在  \src\main\webapp\js\diagramly\Dialogs.js  中  

- splash对话框分析

默认有个splash对话框，查看这个实现  是在 \src\main\webapp\js\diagramly\App.js  

```text
App.prototype.showSplash = function(force)
{

    console.log('show Splash is here');
    //Splash dialog shouldn't be shownn when running without a file menu
    if (urlParams['noFileMenu'] == '1')
    {
        return; 
    }

    var serviceCount = this.getServiceCount(true);

    var showSecondDialog = mxUtils.bind(this, function()
    {
        var dlg = new SplashDialog(this);  //对话框实现在  \src\main\webapp\js\diagramly\Dialogs.js 


var SplashDialog = function(editorUi)  //可以看到 app.js中的this 等价与Dialogs.js 的editorUi 
{
    console.log('dialogs SplashDialog');
    var div = document.createElement('div');
    div.style.textAlign = 'center';
```

是一个原型定义，具体调用代码  

```text
this.showSplash();
```

- 添加默认弹框功能
   再看\src\main\webapp\js\diagramly\App.js  具体实现，在最顶端加入功能代码即可   

```text
this.load(); //加载很多功能代码 

//自加入代码 
if (urlParams['sql'] == '1') //参数中存在 sql=1 则开始弹框 
{
    var dlg = new ParseDialog(this, 'title', 'formatSql');  //弹哪个框 需要指定 第三方参数 
    this.showDialog(dlg.container, 620, 420, true, false);
    this.dialog.container.style.overflow = 'auto';
    dlg.init();
    console.log('app is here');
}
```

这样功能基本就实现了
 但由于默认是有splash弹出框，导致用户打开弹出两个框,因此请求url参数 为    

```text
?splash=0&sql=1
```

### 添加问题反馈

反馈功能使用腾讯的 [兔小巢](https://link.zhihu.com/?target=https%3A//support.qq.com/product/352799) 将右上角的共享功能 修改为 URL共享, 添加一个 问题反馈 按钮  

定位 右上角 共享 功能 代码
 在界面上点击 共享 -> 右键 -> 检查
 在源代码中看到  如下图片地址  

```text
data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8
```

搜索到 在 src\main\webapp\js\diagramly\App.js 中进行 定义   

```text
App.prototype.shareImage =
```

在查看 shareImage 使用位置 定位到   

```text
if (this.shareButton == null)
{
    this.shareButton = document.createElement('div');
    this.shareButton.className = 'geBtn gePrimaryBtn';
    this.shareButton.style.display = 'inline-block';
    this.shareButton.style.backgroundColor = '#F2931E';
    this.shareButton.style.borderColor = '#F08705';
    this.shareButton.style.backgroundImage = 'none';
    this.shareButton.style.padding = '2px 10px 0 10px';
    this.shareButton.style.marginTop = '-10px';
    this.shareButton.style.height = '28px';
    this.shareButton.style.lineHeight = '28px';
    this.shareButton.style.minWidth = '0px';
    this.shareButton.style.cssFloat = 'right';
    this.shareButton.setAttribute('title', mxResources.get('share'));

    var icon = document.createElement('img');
    icon.setAttribute('src', this.shareImage);
    icon.setAttribute('align', 'absmiddle');
    icon.style.marginRight = '4px';
    icon.style.marginTop = '-3px';
    this.shareButton.appendChild(icon);

    if (!Editor.isDarkMode() && uiTheme != 'atlas')
    {
        this.shareButton.style.color = 'black';
        icon.style.filter = 'invert(100%)';
    }

    mxUtils.write(this.shareButton, mxResources.get('share'));

    mxEvent.addListener(this.shareButton, 'click', mxUtils.bind(this, function()
    {
        //  修改为 URL功能 
        // this.actions.get('share').funct();
            this.actions.get('exportUrl').funct();
    }));

    this.buttonContainer.appendChild(this.shareButton);
}

if (this.suggestButton == null)
{
    this.suggestButton = document.createElement('div');
    this.suggestButton.className = 'geBtn gePrimaryBtn';
    this.suggestButton.style.display = 'inline-block';
    this.suggestButton.style.backgroundColor = '#F2931E';
    this.suggestButton.style.borderColor = '#F08705';
    this.suggestButton.style.backgroundImage = 'none';
    this.suggestButton.style.padding = '2px 10px 0 10px';
    this.suggestButton.style.marginTop = '-10px';
    this.suggestButton.style.height = '28px';
    this.suggestButton.style.lineHeight = '28px';
    this.suggestButton.style.minWidth = '0px';
    this.suggestButton.style.cssFloat = 'right';
    this.suggestButton.setAttribute('title', mxResources.get('suggestion'));

    var icon = document.createElement('img');
    icon.setAttribute('src', this.shareImage);
    icon.setAttribute('align', 'absmiddle');
    icon.style.marginRight = '4px';
    icon.style.marginTop = '-3px';
    this.suggestButton.appendChild(icon);

    if (!Editor.isDarkMode() && uiTheme != 'atlas')
    {
        this.suggestButton.style.color = 'black';
        icon.style.filter = 'invert(100%)';
    }

    mxUtils.write(this.suggestButton, mxResources.get('suggestion'));

    mxEvent.addListener(this.suggestButton, 'click', mxUtils.bind(this, function()
    {
        //  修改为 URL功能 
        this.openLink('https://support.qq.com/product/352799');

    }));

    this.buttonContainer.appendChild(this.suggestButton);
}
```

dia.txt 添加  suggestion=Suggestion  

dia_zh.txt 添加  suggestion=反馈问题

### File菜单功能修改

- 删除 synchronize(同步) share...(共享)功能  \src\main\webapp\js\diagramly\Menus.js  注释掉如下代码  

```text
//  var item = this.addMenuItem(menu, 'synchronize', parent);
//  this.addLinkToItem(item, 'https://www.diagrams.net/doc/faq/synchronize');
//  this.addMenuItems(menu, ['share', '-'], parent);
```

功能还在，只是将添加到菜单功能删除  - 删除 嵌入 -> google 相关功能
 做如下代码替换  

```text
//  this.addMenuItems(menu, ['-', 'googleDocs', 'googleSlides', 'googleSheets', '-', 'microsoftOffice', '-', 'embedNotion'], parent);
    this.addMenuItems(menu, ['-',  'embedNotion'], parent);
```

- 删除 导出为 -> pdf 功能  

由于该功能，需要使用官方地址，并且不知道这个具体功能是什么 先关闭   

```text
// this.addMenuItems(menu, ['exportPdf'], parent);
```

### Help菜单修改

删除Fork me 和 获取桌面版本  

```text
/*  this.addMenuItems(menu, ['-', 'keyboardShortcuts', 'quickStart',
                        'support', '-', 'forkme', 'downloadDesktop', '-', 'about'], parent);
                */
                this.addMenuItems(menu, ['-', 'keyboardShortcuts', 'quickStart',
                        'support', '-',  'about'], parent);
```

每次打开页面，还是会提示下载桌面版本，这个功能代码在  

```text
src\main\webapp\js\diagramly\App.js
App.prototype.showDownloadDesktopBanner = function()
{
    /*
    this.showBanner('DesktopFooter', mxResources.get('downloadDesktop'), mxUtils.bind(this, function()
    {
        this.openLink('https://get.diagrams.net/');
    }));
    */
};
```

如果不需要，注释掉代码即可，可以用这个功能做一些其他功能，比如查看教程 

```text
App.prototype.showDownloadDesktopBanner = function()
{
    this.showBanner('DesktopFooter', mxResources.get('quickStart'), mxUtils.bind(this, function()
    {
        this.openLink(window.quickStart_url);
    }));
};
```

在 \src\main\webapp\js\PreConfig.js 添加定义 

```text
window.quickStart_url = window.location.protocol + '//' + window.location.host + '/gif/index.gif';
```

在 dia_zh.txt中 修改 

```text
downloadDesktop= 快速入门视频
```

修改菜单 help -> quickStart  

```text
editorUi.actions.addAction('quickStart...', function()
{
    //editorUi.openLink('https://www.youtube.com/watch?v=Z0D96ZikMkc');
    editorUi.openLink(window.quickStart_url);

});
```

修改菜单 help -> support   

```text
editorUi.actions.addAction('support...', function()
{
    if (EditorUi.isElectronApp)
    {
        //editorUi.openLink('https://github.com/jgraph/drawio-desktop/wiki/Getting-Support');
        editorUi.openLink(window.Support_url);
    }
    else
    {
        //editorUi.openLink('https://github.com/jgraph/drawio/wiki/Getting-Support');
        editorUi.openLink(window.Support_url);
    }
});
```

在 \src\main\webapp\js\PreConfig.js 添加定义 

```text
window.Support_url = 'http://doc.github5.com/drawio/doc/base.html';
```

### 右上角语言列表修改

代码在

```text
src\main\webapp\js\diagramly\Init.js

window.mxLanguageMap = window.mxLanguageMap ||
{
    'i18n': '',
    'id' : 'Bahasa Indonesia',
    'ms' : 'Bahasa Melayu',
    'bs' : 'Bosanski',
    'bg' : 'Bulgarian',
    'ca' : 'Català',
    'cs' : 'Čeština',
    'da' : 'Dansk',
    'de' : 'Deutsch',
    'et' : 'Eesti',
    'en' : 'English',
    'es' : 'Español',
    'eu' : 'Euskara',
    'fil' : 'Filipino',
    'fr' : 'Français',
    'gl' : 'Galego',
    'it' : 'Italiano',
    'hu' : 'Magyar',
    'nl' : 'Nederlands',
    'no' : 'Norsk',
    'pl' : 'Polski',
    'pt-br' : 'Português (Brasil)',
    'pt' : 'Português (Portugal)',
    'ro' : 'Română',
    'fi' : 'Suomi',
    'sv' : 'Svenska',
    'vi' : 'Tiếng Việt',
    'tr' : 'Türkçe',
    'el' : 'Ελληνικά',
    'ru' : 'Русский',
    'sr' : 'Српски',
    'uk' : 'Українська',
    'he' : 'עברית',
    'ar' : '       ',
    'fa' : '     ',
    'th' : 'ไทย',
    'ko' : '한국어',
    'ja' : '日本語',
    'zh' : '简体中文',
    'zh-tw' : '繁體中文'
};

修改为   

window.mxLanguageMap = window.mxLanguageMap ||
{
    'en' : 'English',
    'zh' : '简体中文',
    'zh-tw' : '繁體中文'
};
```

只保留需要的语言即可   

### 修改通知功能

通知路径的定义 

```text
src\main\webapp\js\diagramly\Init.js
window.NOTIFICATIONS_URL = window.NOTIFICATIONS_URL ||  'https://www.draw.io/notifications';

src\main\webapp\js\diagramly\App.js

App.prototype.fetchAndShowNotification = function(target)
{
    if (this.fetchingNotif)
    {
        return; 
    }

修改为 
App.prototype.fetchAndShowNotification = function(target)
{
    if (this.fetchingNotif)
    {

    }
    return;
```

### 首页修改

### 修改菜单搜索

搜索 修改为 百度 src\main\webapp\js\diagramly\Menus.js  

```text
// this.editorUi.openLink('https://www.diagrams.net/search?search=' +
                            encodeURIComponent(term));
//this.editorUi.openLink('https://www.google.com/search?q=site%3Adiagrams.net+inurl%3A%2Fdoc%2Ffaq%2F+' +
            //  encodeURIComponent(term));
this.editorUi.openLink('https://www.baidu.com/s?wd=site%3Agithub5.com+' + encodeURIComponent(term));
```

### 首页报错

有时打开页面会提示一个对话框  Error loading file 和 No file selected
 导致无法使用，根据关键词定位到代码  在  src\main\webapp\js\diagramly\App.js

```text
if (this.editor.isChromelessView())
    {  
        this.handleError({message: mxResources.get('noFileSelected')},
            mxResources.get('errorLoadingFile'), mxUtils.bind(this, function()
        {
            this.showSplash();
        }));
    }
```

但没有搞清楚这个isChromelessView什么时候设置成了true? 先暂且把这段注释掉,但运行一段时间发现又会报错...

最终是\src\main\webapp\js\PreConfig.js

```text
window.EXPORT_URL = 'REPLACE_WITH_YOUR_IMAGE_SERVER';
window.PLANT_URL = 'REPLACE_WITH_YOUR_PLANTUML_SERVER';
window.DRAWIO_BASE_URL = window.location.protocol + '//' + window.location.host; // Replace with path to base of deployment, e.g. https://www.example.com/folder
window.DRAWIO_VIEWER_URL = window.location.protocol + '//' + window.location.host + '/js/viewer.min.js'; // Replace your path to the viewer js, e.g.
window.DRAWIO_LIGHTBOX_URL = null; // window.location.protocol + '//' + window.location.host; // Replace with your lightbox URL, eg. https://www.example.com
window.DRAW_MATH_URL = 'math';
window.DRAWIO_CONFIG = null; // Replace with your custom draw.io configurations. For more details, https://www.diagrams.net/doc/faq/configure-diagram-editor
window.quickStart_url = window.location.protocol + '//' + window.location.host + '/gif/index.gif'; 
window.Support_url = 'http://doc.github5.com/drawio/doc/base.html'; 
urlParams['sync'] = 'manual';
urlParams['lang'] = 'en';
urlParams['browser'] = 1;
urlParams['gapi'] = 0;
urlParams['db'] = 0;
urlParams['od'] = 0; 
urlParams['tr'] = 0;
urlParams['gh'] =0; 
urlParams['gl'] =0;
urlParams['mode'] = 'browser';
```

window.DRAWIO_LIGHTBOX_URL  不能被改, 必须是 window.DRAWIO_LIGHTBOX_URL =null 

分析原因:  \src\main\webapp\js\diagramly\Init.js  使用这个变量   

```text
// Uses lightbox mode on viewer domain
if (window.location.hostname == DRAWIO_LIGHTBOX_URL.substring(DRAWIO_LIGHTBOX_URL.indexOf('//') + 2))
{
    urlParams['lightbox'] = '1';
}   

// Lightbox enables chromeless mode
if (urlParams['lightbox'] == '1')
{
    urlParams['chrome'] = '0';
}
```

会进入lightbox 模式,官方解释:  

```text
lightbox=1: Uses the lightbox in chromeless mode (larger zoom, no page visible, chromeless).
```

所以先将 

```text
urlParams['lightbox'] = '1';
```

注释掉 

## 部署上线

终于可以上线了 

最终[在线画图地址](https://link.zhihu.com/?target=http%3A//tu.github5.com/) 

js目录的子文件夹都可以删除 

## TODO

未完成的事业   

-  插件等功能学习 
   
-  支持服务端存储  
   
-  高级功能教程  
   

## 交流

[drawio二次开发交流](https://link.zhihu.com/?target=http%3A//doc.github5.com/drawio/develop.html)

## 参考文档

[drawio 官方文档](https://link.zhihu.com/?target=https%3A//www.diagrams.net/doc/)

[drawio 二次开发优秀项目](https://link.zhihu.com/?target=http%3A//processflow.sharehoo.cn/)

[drawio 编码和解密工具](https://link.zhihu.com/?target=https%3A//jgraph.github.io/drawio-tools/tools/convert.html)

[drawio 使用教程](https://zhuanlan.zhihu.com/p/129659662)

[drawio 画流程图](https://link.zhihu.com/?target=https%3A//blog.csdn.net/zyx_ly/article/details/102488457)

