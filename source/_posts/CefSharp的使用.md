---
title: CefSharp的使用
author: HongChangCui
date: 2018-03-11
categories: 开源项目
tags:
	- CEF
	- CefSharp
cover_picture: Cef.jpg
---

CEF（Chrominu Embedded Framework）:基于Google Chrominu项目的一个开源项目，但与Google Chrominu项目不同，Google Chrominu关注于Google Chrome应用程序开发，而CEF关注于促进在第三方应用中嵌入浏览器。CEF支持各种语言的程序和操作系统，并能够很容易的与应用程序进行集成。

官网（[https://bitbucket.org/chromiumembedded/cef](https://bitbucket.org/chromiumembedded/cef)）
<!--more-->

## CefSharp简介
CefSharp可以在.NET 应用程序嵌入谷歌浏览器。介绍:（[https://github.com/cefsharp/CefSharp](https://github.com/cefsharp/CefSharp)）
## CefSharp示例
CefSharp提供了WPF和WinForm的web browser控件的实现。

具体Demo:
CefSharp.Wpf:[http://www.nuget.org/packages/CefSharp.Wpf/](http://www.nuget.org/packages/CefSharp.Wpf/)
CefSharp.WinForms:[http://www.nuget.org/packages/CefSharp.WinForms/](http://www.nuget.org/packages/CefSharp.WinForms/)

## CefSharp引用
通过Nuget控制台管理工具在项目中（WPF或者WinForm）引入所需要的包。
注意：如果不指定版本：Install-Package CefSharp.Wpf ，该命令下载的是最新包。不同的版本的包会有不同改变和要求。

如现在最新版本为v57.0.0，在版本说明中（详见：[https://github.com/cefsharp/CefSharp/releases/tag/v57.0.0](https://github.com/cefsharp/CefSharp/releases/tag/v57.0.0)）

**Breaking Changes:“CefSharp requires at least .Net 4.5.2 (Last version to support .Net 4.0 is 49)”,**说明了其需求。

## CefSharp使用【WPF】

### 初始化Chrome

```c#
var setting = new CefSharp.CefSettings();
CefSharp.Cef.Initialize(setting,true,false);
```

setting变量是用来存放Chrome全局设置的地方，当需要进行设置时，只需要对其进行修改，如，修改缓存目录：
```c#
var setting = new CefSharp.CefSettings()
{
   CachePah = Directory.GetCurrentDirectory() + @"\Cache",
};
```
### 实例化webBroswer

```c#
CefSharp.Wpf.ChromiumWebBrowser webView = new CefSharp.Wpf.ChromiumWebBrowser();
```

### 指定显示画面Address
```c#
webView.Address = "xxxxxxxxx";
```

注意：webView加载到具体窗体上才能调用，否则调用js会出错：IBroswer为null，如WPF或者WinForm,可以在窗体页面上添加该webView;

### CefSharp与画面交互

#### CefSharp对象调JS

ExecuteScriptAsync:执行js，没有返回值

```c#
webview.ExecuteScriptAsync("yyy()");//yyy是JS的方法名
```

EvaluateScriptAsync:执行js，有返回值【返回对象：CefSharp.JavaScriptResponse,返回内容：对象.Result.Result】

```c#
Task<CefSharp.JavaScriptResponse> t = webView.EvaluateScriptAsync("yyy()");//yyy是JS方法名
```

问题：调用js方法，如何传参数？---**参数只能是字符串？？？**

- 单个字符串
```c#
string name="chc";  
**yyy("\""+name+"\""+")**
```

- 多个值，使用JSON转换成一个字符串

```c#
string response = JsonConvert.SerializeObject(new
{
    name = "cuihongchang",
    sex = "man"
});

yyy("+reponse+")  //js端会自动解析成对象，直接调用即可，如data.name等
```

#### JS调CefSharp对象
RegisterJsObject：将某个对象注册成js对象，可以在js画面上直接调用该对象的属性和方法。

```c#
public class JsEvent
{
	public string MessageText = string.Empty;
	public void ShowTest()
	{
		MessageBox.Show("this in C#.\n\r" + MessageText);
	}
}

webView.RegisterJsObject("jsObj",new JsEvent(),false);
```
在js画面就可以调用：

```javascript
jsObj.MessageText = "hello";
jsObj.ShowTest(); 
```

问题：

**1.Browser is already initialized. RegisterJsObject must be called before the underlying CEF browser is created。**

只能选择在代码中加载的方式，而不能使用控件方式，如下：【**后来发现不是这样，I don't know why**】

```c#
public void Initialize()
{
	if (!Cef.Initialize(settings)) return;

	ChromiumWebBrowser cefWebView = new ChromiumWebBrowser();

	//注册类到js中
	cefWebView.RegisterJsObject("WPFComm", new JSDoor(this));

	BrowserContainer.Children.Add(cefWebView);

	cefWebView.Load(App.Config.StartPage);
}
```
**2.注册js对象时，注意第三个参数，默认为true，表示调用的方法采用驼峰命名法，即首字母小写，反之，采用帕斯卡命名法，首字母大写**
## CefSharp总结

### CefSharp初始化

###### 利用控件

```xml
<wpf:ChromiumWebBrowser x:Name="Browser"
Address="{Binding Text, ElementName=txtBoxAddress}">
  
</wpf:ChromiumWebBrowser>
```

###### 利用代码
```c#
public void Initialize()
{
	if (!Cef.Initialize(settings)) return;
	ChromiumWebBrowser cefWebView = new ChromiumWebBrowser();
	BrowserContainer.Children.Add(cefWebView);
}
```
### CefSharp常用按钮

```c#
//调试
webView.GetBrowser().ShowDevTools();
//刷新C
webView.GetBrowser().Reload();
//上一页
webView.GetBrowser().GoBack();
//下一页
webView.GetBrowser().GoForward();
```

### CefSharp资源清理

```c#
//浏览器本身处理
static ChromiumWebBrowser()  
{  
    if (CefSharpSettings.ShutdownOnExit)  
    {  
    	Application.ApplicationExit += OnApplicationExit;  
    }  
}  

private static void OnApplicationExit(object sender, EventArgs e)  
{  
	Cef.Shutdown();  
}  

//需要关闭浏览器负载程序时操作
try  
{  
    browser.CloseDevTools();  
    browser.GetBrowser().CloseBrowser(true);  
}  
catch { }   

try  
{  
    if (browser != null)  
    {  
	    browser.Dispose();  
	    Cef.Shutdown();  
    }  
}  
catch { }  
```