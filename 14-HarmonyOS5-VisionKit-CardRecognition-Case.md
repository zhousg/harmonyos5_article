# HarmonyOS 5.0 中 VisionKit 证件识别案例

## 内容摘要
本文介绍了在 HarmonyOS 5.0 里如何使用 `@kit.VisionKit` 实现证件识别功能。通过创建 `VisionKitCardRecognition` 组件，用户可以进行身份证识别，并展示识别结果。

## 实现步骤
1. 导入 `@kit.VisionKit` 和 `@kit.ArkUI` 中的相关模块。
```typescript
import { CardRecognition, CardType, CardSide, ShootingMode } from "@kit.VisionKit" 
import { router } from "@kit.ArkUI"; 
```
2. 定义 `VisionKitCardRecognition` 组件，设置证件数据状态。
```typescript
@Entry 
@Component 
struct VisionKitCardRecognition { 
  @State cardDataSource: Record<string, string>[] = [] 
}
```
3. 构建 UI 界面，包含证件数据展示区域和 `CardRecognition` 组件。
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
4. 实现 `CardRecognition` 组件的回调函数，处理识别结果并更新证件数据状态。
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
5. 实现 `cardDataShowBuilder` 方法，用于展示证件数据。
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

## 完整示例代码
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

## 总结
本案例展示了在 HarmonyOS 5.0 中使用 `@kit.VisionKit` 实现证件识别的方法。关键知识点包括 `CardRecognition` 组件的配置和使用、识别结果的处理，以及证件数据的展示。这些方法可用于开发各类需要证件识别功能的应用。

## 适用场景

卡证识别控件提供身份证（目前仅支持中国）、行驶证、驾驶证、护照、银行卡的结构化识别服务，满足卡证的自动分类功能，系统可自动判断所属卡证类型并返回结构化信息和卡证图片信息。

对于需要填充卡证信息的场景，如身份证、银行卡信息等，可使用卡证识别控件读取OCR（Optical Character Recognition）信息，将结果信息返回后进行填充。支持单独识别正面、反面，或同时进行双面识别。


## 约束限制

支持的语种类型：简体中文、英文。
卡证识别暂时只支持身份证、行驶证、驾驶证、护照、银行卡5种卡证。
不允许被其他组件或窗口遮挡。