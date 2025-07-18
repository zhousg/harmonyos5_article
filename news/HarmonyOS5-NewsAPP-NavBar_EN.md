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

## Expansion
### Custom Components
In ArkUI, all UI display content consists of components. Those directly provided by the framework are called system components, while those defined by developers are called custom components. When developing UI interfaces, you need to not only combine system components but also consider code reusability, separation of business logic and UI, and the evolution of subsequent versions. Therefore, encapsulating UI and part of the business logic into custom components is an indispensable capability.

### Custom Builder Functions
ArkUI provides a lightweight UI element reuse mechanism `@Builder`, whose internal UI structure is fixed and only performs data transfer with the user. Developers can abstract frequently used UI elements into functions and call them in the `build` function.

### What are the differences and limitations between the custom builder function `@Builder` and the custom component `@Component`?
The main differences between the custom builder function `@Builder` and the custom component `@Component` are as follows:

The custom builder function `@Builder` is lighter. As a method for UI element abstraction, its implementation and invocation are simpler than those of custom components.
In a custom component, you can define member functions, variables, and the component's lifecycle.
The custom builder function `@Builder` does not support defining state variables and lifecycle.

In a custom component, changes to state variables can directly drive UI refresh.
However, the default value - passing parameter method of the custom builder function `@Builder` does not support dynamically changing components. When the passed parameter is a state variable, changes to the state variable will not cause the UI within the `@Builder` method to refresh. To achieve dynamic UI refresh, you need to pass parameters by reference.
To implement the slot function in a custom component, you need to use `@Builder` and `@BuilderParam`.
For specific implementation, refer to the `@BuilderParam` decorator: Reference the `@Builder` function.

If a custom component is used in the custom builder function `@Builder`, the corresponding custom component will be re - created each time the method is called.
```