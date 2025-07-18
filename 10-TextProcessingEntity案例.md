# 基于 HarmonyOS 的文本处理实体提取案例

## 内容摘要
本文介绍了如何在 HarmonyOS 中使用 `@kit.NaturalLanguageKit` 从文本中提取电子邮件地址。通过创建一个 `TextProcessingEntity` 组件，用户可以输入包含电子邮件地址的文本，点击按钮提取电子邮件地址，最后显示提取的电子邮件地址。

## 实现步骤
1. 从 `@kit.NaturalLanguageKit` 导入 `textProcessing` 模块和 `EntityType`。
2. 创建一个 `TextProcessingEntity` 组件，该组件包含一个文本输入框、一个用于显示提取的电子邮件地址的文本区域和一个提取按钮。
3. 实现按钮点击的事件处理函数，并调用 `textProcessing.getEntity` 从文本中提取实体。
4. 过滤提取的实体，找到电子邮件地址并显示它。

## 落地代码
```typescript
import { EntityType, textProcessing } from '@kit.NaturalLanguageKit'; 

@Entry 
@Component 
struct TextProcessingEntity { 
  @State article: string = '电子邮件（EMAIL）：识别文本中的电子邮件地址,如“example@abc.com”'; 
  @State email: string = '' 

  build() { 
    Column({ space: 20 }) { 
      TextArea({ text: this.article }) 
        .height(200) 
      Text('电子邮箱：' + this.email) 
      Button('提取邮箱') 
        .onClick(async () => { 
          const results = await textProcessing.getEntity(this.article) 
          results.forEach(item => { 
            if (item.type === EntityType.EMAIL) { 
              this.email = item.text 
            } 
          }) 
        }) 
    } 
    .padding(15) 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## 总结
本案例展示了在 HarmonyOS 中使用 `@kit.NaturalLanguageKit` 进行实体提取的方法。通过创建组件并使用 `textProcessing.getEntity` 方法，我们实现了从文本中提取电子邮件地址的功能。关键知识点包括 HarmonyOS 组件的创建、状态管理、异步函数的使用以及实体提取。这种方法可以作为基础，扩展更多复杂的自然语言处理功能。

## 适用场景

适用场景
实体抽取是自然语言处理服务的一项关键能力。以综合上下文信息，从文本中准确识别出多种类型的实体：

日期时间（DATETIME）：提取文本中的具体日期,如“2024年6月18日”等。
电子邮件（EMAIL）：识别文本中的电子邮件地址,如“example@abc.com”。
快递单号（EXPRESS_NO）：抽取文本中的快递单号信息。
航班号（FLIGHT_NO）：定位文本中的航班号,如“CA1234”等。
地址（LOCATION）：从文本中提取详细的地址描述。
人名（NAME）：找出文本中出现的人名信息。
手机号（PHONE_NO）：识别文本中的手机号码。
网址（URL）：抽取文本中的网址链接。
验证码（VERIFICATION_CODE）：定位文本中的验证码数字。
身份证号（ID_NO）：识别文本中的身份证号码信息。
通过准确抽取以上多种类型的实体信息，该项能力可以广泛应用于新闻阅读、信息检索、客户服务、社交聊天、金融运营等多种场景。例如，在新闻阅读场景中，可以对新闻正文进行实体抽取，并对人名、地名、时间、网址等关键实体信息进行高亮标识，帮助读者快速获取文章要点；在客服场景，通过抽取用户留言中的手机号、快递单号、验证码等信息，客服人员能够更高效地定位问题并给出解决方案。实体抽取为各行业的智能化应用提供了坚实的基础支持。

## 约束限制

支持的语言：简体中文、英文、繁体中文。
支持的实体：时间、地点、邮箱、快递单号、航班号、人名、电话号码、网址链接、验证码、证件号。
文本长度：不超过1000字符。