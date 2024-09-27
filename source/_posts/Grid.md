---
title: Grid横向滚动视图
date: 2024-09-27 10:53:20
tags:
---
### Grid
#### 自定义组件-构建单个item视图
- 网格容器，由“行”和“列”分割的单元格所组成，通过指定“项目”所在的单元格做出各种各样的布局。
#### 子组件
 **包含GridItem子组件。** 
 **说明** 

- Grid子组件的索引值计算规则：
- 
- 按子组件的顺序依次递增。
- if/else语句中，只有条件成立分支内的子组件会参与索引值计算，条件不成立分支内的子组件不计算索引值。
- ForEach/LazyForEach语句中，会计算展开所有子节点索引值。
- if/else/ForEach/LazyForEach发生变化以后，会更新子节点索引值。
- Grid子组件的visibility属性设置为Hidden或None时依然会计算索引值。
- Grid子组件的visibility属性设置为None时不显示，但依然会占用子组件对应的网格。
- Grid子组件设置position属性，会占用子组件对应的网格，子组件将显示在相对Grid左上角偏移position的位置。该子组件不会随其对应网格滚动，在对应网格滑出Grid显示范围外后不显示。
- 当Grid子组件之间留有空隙时，会根据当前的展示区域尽可能填补空隙，因此GridItem可能会随着网格滚动而改变相对位置。
#### 这里我们将Row、Column、Text、Image组件组合成为一个新的模块，案例代码如下：

```

 build() {
    Column() {
      Image(this.enablementItem.imageSrc)
        .width('100%')
        .objectFit(ImageFit.Cover)
        .height(96)
        .borderRadius({
          topLeft: 16,
          topRight: 16
        })
      Text(this.enablementItem.title)
        .height(19)
        .width('100%')
        .fontSize(14)
        .textAlign(TextAlign.Start)
        .maxLines(1)
        .fontWeight(400)
        .padding({
          left: 12,
          right: 12
        })
        .margin({top: 8})
        .textOverflow({overflow: TextOverflow.Ellipsis})
      Text(this.enablementItem.brief)
        .height(32)
        .width('100%')
        .fontSize(12)
        .textAlign(TextAlign.Start)
        .maxLines(2)
        .fontWeight(400)
        .padding({
          left: 12,
          right: 12
        })
        .margin({top: 2})
        .textOverflow({overflow: TextOverflow.Ellipsis})
        .fontColor('rgba(0,0,0,0.6)')
    }
    .width(160)
    .height(169)
    .borderRadius(16)
    .backgroundColor(Color.White)
  }
```
### 父组件中数据导入到子组件中
 **使用@Prop装饰器：父子单向同步** 

- @Prop装饰的变量可以和父组件建立单向的同步关系。@Prop装饰的变量是可变的，但是变化不会同步回其父组件。
- 
- @Prop装饰的变量和父组件建立单向的同步关系：
- 
- @Prop变量允许在本地修改，但修改后的变化不会同步回父组件。
- 当数据源更改时，@Prop装饰的变量都会更新，并且会覆盖本地所有更改。因此，数值的同步是父组件到子组件（所属组件)，子组件数值的变化不会同步到父组件。

