# HarmonyOS5 新闻应用导航栏实现

## 内容摘要
本文介绍 HarmonyOS 5.0 新闻应用中导航栏的实现方法，使用 ArkTS 语言实现 `NavBarBuilder` 和 `BarButton` 组件，实现导航栏的布局和样式控制。

## 实现步骤
1. 定义 `NavBarBuilder` 构建器
2. 定义 `BarButton` 组件
3. 配置按钮样式和布局

## 落地代码
### 1. 定义 `NavBarBuilder` 构建器
```typescript
@Builder 
NavBarBuilder() { 
  Row({ space: 10 }) { 
    BarButton({ icon: $r('sys.media.ohos_ic_public_email') , type: 'normal' }) 
    Blank() 
    BarButton({ icon: $r('sys.media.ohos_ic_public_search_filled') , type: 'normal' }) 
    BarButton({ icon: $r('sys.media.ohos_ic_public_clock') , type: 'normal' }) 
  } 
  .padding(15) 
  .width('100%') 
}
```

### 2. 定义 `BarButton` 组件
```typescript
@Component 
struct BarButton { 
  icon: ResourceStr = '' 
  type: 'transparent' | 'normal' = 'transparent' 

  build() { 
    Row() { 
      Image(this.icon) 
        .width(24) 
        .height(24) 
        .fillColor(this.type === 'transparent' ? Color.White : Color.Black) 
    } 
    .justifyContent(FlexAlign.Center) 
    .width(40) 
    .aspectRatio(1) 
    .borderRadius(22) 
    .backgroundColor(this.type === 'transparent' ? '#45FFFFFF' : '#f3f4f5') 
  } 
}
```

## 总结
关键知识点包括 `@Builder` 装饰器的使用、组件定义、布局容器 `Row` 的使用，以及样式属性的配置。通过 `NavBarBuilder` 和 `BarButton` 组件的组合，实现了灵活可配置的导航栏。

## 扩展

### 自定义组件

在ArkUI中，UI显示的内容均为组件，由框架直接提供的称为系统组件，由开发者定义的称为自定义组件。进行UI界面开发时，不仅要组合使用系统组件，还需考虑代码的可复用性、业务逻辑与UI的分离，以及后续版本的演进等因素。因此，将UI和部分业务逻辑封装成自定义组件是不可或缺的能力。

### 自定义构建函数

ArkUI提供轻量的UI元素复用机制@Builder，其内部UI结构固定，仅与使用方进行数据传递。开发者可将重复使用的UI元素抽象成函数，在build函数中调用。

### 自定义构建函数Builder与自定义组件component的使用区别以及限制是什么?

自定义构建函数（@Builder）和自定义组件的主要区别包括：

自定义构建函数（@Builder）更轻量。它作为UI元素抽象的方法，实现和调用比自定义组件更简洁。
在自定义组件中，可以定义成员函数和变量，以及组件的生命周期。
自定义构建函数（@Builder）不支持定义状态变量和生命周期。

在自定义组件中，状态变量的改变可直接驱动UI刷新。
而自定义构建函数（@Builder）默认的按值参数传递方式不支持动态改变组件，当传递的参数为状态变量时，状态变量的改变不会引起@Builder方法内的UI刷新，要实现UI动态刷新需要按引用传递参数。

在自定义组件中实现插槽功能，需使用@Builder和@BuilderParam。
具体实现可参考：@BuilderParam装饰器：引用@Builder函数。

在自定义构建函数（@Builder）中使用了自定义组件，每次调用该方法时，对应的自定义组件都会重新创建。