# HarmonyOS 5 News App Navigation Bar Implementation

## Abstract
This article introduces the implementation of the navigation bar in the HarmonyOS 5.0 news application. It uses the ArkTS language to implement the `NavBarBuilder` and `BarButton` components to control the layout and style of the navigation bar.

![img05.png](img05.png)

## Implementation Steps
1. Define the `NavBarBuilder` builder.
2. Define the `BarButton` component.
3. Configure button styles and layouts.

## Code Implementation
### 1. Define the `NavBarBuilder` Builder
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

### 2. Define the `BarButton` Component
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

## Summary
Key knowledge points include the use of the `@Builder` decorator, component definition, the use of the `Row` layout container, and the configuration of style attributes. By combining the `NavBarBuilder` and `BarButton` components, a flexible and configurable navigation bar is implemented.