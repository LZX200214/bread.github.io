---
title: 轮播图实现-Swiper控件的使用
date: 2024-09-27 10:52:29
tags:
---
## 1. Swiper组件
Swiper组件提供滑动轮播显示的能力，针对负载页面场景，可以使用Swiper组件的预加载机制，利用主线程的空闲时间来提前构建和部署绘制组件。

## 2. Swiper组件使用案例：

```
 Swiper() {
      Text($r('app.string.hello_message'))
        .fontSize(25)
        .margin(30)
        .fontWeight(FontWeight.Bold)
      Text($r('app.string.class_message'))
        .fontSize(25)
        .margin(30)
        .fontWeight(FontWeight.Bold)

    }
    .autoPlay(true)
    .loop(true)

  }
```
![输入图片说明](https://foruda.gitee.com/images/1726192642752557611/c8b3ea04_14874917.png "屏幕截图")

## 3. ForEach 循环渲染
## 首次渲染
ForEach接口基于数组类型数据来进行循环渲染，需要与容器配合使用，且接口返回的组件应当是允许包括在ForEach父容器中的子组件。

```
@Entry
@Component
struct Parent {
  @State simpleList: Array<string> = ['one', 'two', 'three'];

  build() {
    Row() {
      Column() {
        ForEach(this.simpleList, (item: string) => {
          ChildItem({ item: item })
        }, (item: string) => item)
      }
      .width('100%')
      .height('100%')
    }
    .height('100%')
    .backgroundColor(0xF1F3F5)
  }
}

@Component
struct ChildItem {
  @Prop item: string;

  build() {
    Text(this.item)
      .fontSize(50)
  }
}
```
ForEach数据源不存在相同值案例首次渲染运行效果图
![输入图片说明](https://foruda.gitee.com/images/1726194167919953465/577572af_14874917.png "屏幕截图")
在上述代码中，键值生成规则是keyGenerator函数的返回值item。在ForEach渲染循环时，为数据源数组项依次生成键值one、two和three，并创建对应的ChildItem组件渲染到界面上。

当不同数组项按照键值生成规则生成的键值相同时，框架的行为是未定义的。例如，在以下代码中，ForEach渲染相同的数据项two时，只创建了一个ChildItem组件，而没有创建多个具有相同键值的组件。

```
@Entry
@Component
struct Parent {
  @State simpleList: Array<string> = ['one', 'two', 'two', 'three'];

  build() {
    Row() {
      Column() {
        ForEach(this.simpleList, (item: string) => {
          ChildItem({ item: item })
        }, (item: string) => item)
      }
      .width('100%')
      .height('100%')
    }
    .height('100%')
    .backgroundColor(0xF1F3F5)
  }
}

@Component
struct ChildItem {
  @Prop item: string;

  build() {
    Text(this.item)
      .fontSize(50)
  }
}

```
ForEach数据源存在相同值案例首次渲染运行效果图
![输入图片说明](https://foruda.gitee.com/images/1726194232335199331/fb99dd21_14874917.png "屏幕截图")
在该示例中，最终键值生成规则为item。当ForEach遍历数据源simpleList，遍历到索引为1的two时，按照最终键值生成规则生成键值为two的组件并进行标记。当遍历到索引为2的two时，按照最终键值生成规则当前项的键值也为two，此时不再创建新的组件。

## 非首次渲染
在ForEach组件进行非首次渲染时，它会检查新生成的键值是否在上次渲染中已经存在。如果键值不存在，则会创建一个新的组件；如果键值存在，则不会创建新的组件，而是直接渲染该键值所对应的组件。例如，在以下的代码示例中，通过点击事件修改了数组的第三项值为"new three"，这将触发ForEach组件进行非首次渲染。

```
@Entry
@Component
struct Parent {
  @State simpleList: Array<string> = ['one', 'two', 'three'];

  build() {
    Row() {
      Column() {
        Text('点击修改第3个数组项的值')
          .fontSize(24)
          .fontColor(Color.Red)
          .onClick(() => {
            this.simpleList[2] = 'new three';
          })

        ForEach(this.simpleList, (item: string) => {
          ChildItem({ item: item })
            .margin({ top: 20 })
        }, (item: string) => item)
      }
      .justifyContent(FlexAlign.Center)
      .width('100%')
      .height('100%')
    }
    .height('100%')
    .backgroundColor(0xF1F3F5)
  }
}

@Component
struct ChildItem {
  @Prop item: string;

  build() {
    Text(this.item)
      .fontSize(30)
  }
}
```
ForEach非首次渲染案例运行效果图
![输入图片说明](https://foruda.gitee.com/images/1726194755480575057/4eaee5dc_14874917.png "屏幕截图")
从本例可以看出@State 能够监听到简单数据类型数组数据源 simpleList 数组项的变化。
1.当 simpleList 数组项发生变化时，会触发 ForEach 进行重新渲染。
2.ForEach 遍历新的数据源 ['one', 'two', 'new three']，并生成对应的键值one、two和new three。
3.其中，键值one和two在上次渲染中已经存在，所以 ForEach 复用了对应的组件并进行了渲染。对于第三个数组项 "new three"，由于其通过键值生成规则 item 生成的键值new three在上次渲染中不存在，因此 ForEach 为该数组项创建了一个新的组件。

## 4. 利用Swiper组件，以及ForEach接口，完成轮播图的填写
#### 思路：

-  由于在页面中的每一个轮播图，实际上包含着图片id，图片加载地址，图片点击后跳转的地址，这三个主要属性，所以我们先将轮播图的特点归纳成为一个统一的轮播图类。创建轮播图类：


```
class BannerClass {
  id: string='';
  imageSRC: ResourceStr = '';
  url: string='';

  constructor(id: string, imageSrc: ResourceStr, url: string) {
    this.id = id;
    this.imageSRC = imageSrc
    this.url = url
  }
}
```
 **相关知识点：** TypeScript中类的创建 

-  **创建数组类型的轮播图数据，由于项目中有6个轮播图，每个轮播图代表着一个轮播图对象（轮播图对象包含id、图片加载地址、图片点击后跳转地址），所以我们要创建6个轮播图对象，并且将这6个对象保存在数据中。** 

```
 @State bannerList:Array<BannerClass> = [
    new BannerClass('pic0',$r('app.media.banner_pic0'),''),
    new BannerClass('pic1',$r('app.media.banner_pic1'),''),
    new BannerClass('pic2',$r('app.media.banner_pic2'),''),
    new BannerClass('pic3',$r('app.media.banner_pic3'),''),
    new BannerClass('pic4',$r('app.media.banner_pic4'),''),
    new BannerClass('pic5',$r('app.media.banner_pic5'),'')
  ];

```

 **相关知识点：** TypeScript中类的数据

-  **由于是多图片轮播效果，这里我们使用Swiper组件实现效果，我们使用ForEach接口循环渲染上一步创建的轮播图数组数据，将多个轮播图渲染进Swiper组件中，并设置Swiper组件的loop属性为true，autoplay属性为true，达到自动循环轮播效果。**  


```
 Swiper() {
      ForEach(this.bannerList,(item: BannerClass, index: number) => {
        Image(item.imageSRC)
          .objectFit(ImageFit.Contain)
          .width('100%')
          .padding({top: 11, left:16, right:36,})
          .borderRadius(16)
      },(item: BannerClass, index: number) => item.id)
    }
    .autoPlay(true)
    .loop(true)
    .indicator(
      new DotIndicator()
        .color('#1a000000')
        .selectedColor('0A59F7')

```







