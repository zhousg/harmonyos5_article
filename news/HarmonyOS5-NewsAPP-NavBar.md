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