# Case Study of VisionKit Card Recognition in HarmonyOS 5.0

## Abstract
This article introduces how to use `@kit.VisionKit` to implement the card recognition function in HarmonyOS 5.0. By creating the `VisionKitCardRecognition` component, users can perform ID card recognition and display the recognition results.

## Implementation Steps
1. Import relevant modules from `@kit.VisionKit` and `@kit.ArkUI`.
```typescript
import { CardRecognition, CardType, CardSide, ShootingMode } from "@kit.VisionKit" 
import { router } from "@kit.ArkUI"; 
```
2. Define the `VisionKitCardRecognition` component and set the card data state.
```typescript
@Entry 
@Component 
struct VisionKitCardRecognition { 
  @State cardDataSource: Record<string, string>[] = [] 
}
```
3. Build the UI interface, including the card data display area and the `CardRecognition` component.
```typescript
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
          }
        })
      } 
      .width('100%') 
      .height('100%') 
    } 
    .width('100%') 
    .height('100%') 
    .hideTitleBar(true) 
  }
```
4. Implement the callback function of the `CardRecognition` component to handle the recognition results and update the card data state.
```typescript
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
```
5. Implement the `cardDataShowBuilder` method to display the card data.
```typescript
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
```

## Complete Example Code
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
This case demonstrates how to use `@kit.VisionKit` to implement card recognition in HarmonyOS 5.0. Key knowledge points include the configuration and use of the `CardRecognition` component, the processing of recognition results, and the display of card data. These methods can be used to develop various applications that require card recognition functions.

## Applicable Scenarios
The card recognition control provides structured recognition services for ID cards (currently only supporting those issued in China), driving licenses, passports, and bank cards. It meets the automatic classification function of cards, allowing the system to automatically determine the type of the card and return structured information and card image information.

For scenarios where card information needs to be filled, such as ID card and bank card information, you can use the card recognition control to read OCR (Optical Character Recognition) information. After the result information is returned, it can be filled in. It supports single - side (front or back) recognition or double - side recognition simultaneously.

## Constraints
- Supported languages: Simplified Chinese, English.
- Currently, card recognition only supports 5 types of cards: ID cards, driving licenses, passports, and bank cards.
- It is not allowed to be blocked by other components or windows.