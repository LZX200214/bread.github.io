---
title: Web页面
date: 2024-09-27 10:53:20
tags:
---
在鸿蒙（HarmonyOS）系统中，WebView组件的使用提供了在应用中集成Web页面的能力，使得开发者可以在HarmonyOS应用程序中嵌入Web内容，实现更丰富的用户界面和功能。以下是关于HarmonyOS中WebView组件使用的详细指南：

### 一、基本使用

#### 1. 配置网络权限

在使用WebView加载网络页面之前，需要确保应用已经配置了网络权限。这通常在应用的`module.json5`文件中进行配置：
![输入图片说明](https://foruda.gitee.com/images/1727579495528017778/7d693637_14874917.png "屏幕截图")

```
"requestPermissions": [{
       "name": "ohos.permission.INTERNET"
    }],
```

#### 2. 加载Web页面

WebView可以加载网络页面或本地页面。加载网络页面的基本方法如下：


```
import { webview } from '@kit.ArkWeb'
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebContentPage {
  controller: webview.WebviewController = new webview.WebviewController();
  build() {
    Column() {
      Button('加载Url')
        .onClick(() => {
          try {
            this.controller.loadUrl('www.baidu.com')
          } catch (error) {
            let e: BusinessError = error as BusinessError;
            console.error(`ErrorCode: ${e.code}, Message: ${e.message}`)
          }
        })
      Web({ src: 'lzx.unitechshowcase.online', controller: this.controller })
      //   lzx.unitechshowcase.online
    }
    .height('100%')
    .width('100%')
  }
}


```
