# Case Study of Card Recognition in VisionKit on HarmonyOS 5.0

## Abstract
This article introduces how to use `@kit.VisionKit` to implement the card recognition function in HarmonyOS 5.0. By creating the `VisionKitCardRecognition` component, users can perform ID card recognition and display the recognition results.

## Implementation Steps
1. Import relevant modules from `@kit.VisionKit` and `@kit.ArkUI`.
2. Define the `VisionKitCardRecognition` component and set the card data state.
3. Build the UI interface, including the card data display area and the `CardRecognition` component.
4. Implement the callback function of the `CardRecognition` component to handle the recognition results and update the card data state.
5. Implement the `cardDataShowBuilder` method to display the card data.

## Implementation Code
```typescript
import { CardRecognition, CardType, CardSide, ShootingMode } from "@kit.VisionKit" 
import { router } from "@kit.ArkUI"; 

@Entry 
@Component 
struct VisionKitCardRecognition { 
  @State cardDataSource: Record<string, string>[] = [] 

  build() { 
    NavDestination() { 
      Stack({ alignContent: Alignment.Top }) { 
        Stack() { 
          this.cardDataShowBuilder() 
        } 
        .width('80%') 
        .height('80%') 


        CardRecognition({ 
          supportType: CardType.CARD_ID, 
          cardSide: CardSide.DEFAULT, 
          cardRecognitionConfig: { 
            defaultShootingMode: ShootingMode.MANUAL, 
            isPhotoSelectionSupported: true 
          }, 
          callback: ((params) => { 
            if (params.code !== 200) { 
              router.back() 
            } 
            if (params.cardInfo?.front !== undefined) { 
              this.cardDataSource.push(params.cardInfo?.front) 
            } 
            if (params.cardInfo?.back !== undefined) { 
              this.cardDataSource.push(params.cardInfo?.back) 
            } 
            if (params.cardInfo?.main !== undefined) { 
              this.cardDataSource.push(params.cardInfo?.main) 
            } 
          }) 
        }) 
      } 
      .width('100%') 
      .height('100%') 
    } 
    .width('100%') 
    .height('100%') 
    .hideTitleBar(true) 
  } 


  @Builder 
  cardDataShowBuilder() { 
    List() { 
      ForEach(this.cardDataSource, (cardData: Record<string, string>) => { 
        ListItem() { 
          Column() { 
            Image(cardData.cardImageUri) 
              .objectFit(ImageFit.Contain) 
              .width(100) 
              .height(100) 


            Text(JSON.stringify(cardData)) 
              .width('100%') 
              .fontSize(12) 
          } 
        } 
      }) 
    } 
    .listDirection(Axis.Vertical) 
    .alignListItem(ListItemAlign.Center) 
    .margin({ 
      top: 50 
    }) 
    .width('100%') 
    .height('100%') 
  } 
} 
```

## Summary
This case demonstrates how to use `@kit.VisionKit` to implement card recognition in HarmonyOS 5.0. The key knowledge points include the configuration and use of the `CardRecognition` component, the handling of recognition results, and the display of card data. These methods can be used to develop various applications that require card recognition functions.