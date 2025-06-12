# Text Processing Entity Extraction Case on HarmonyOS

## Summary
This article introduces how to use `@kit.NaturalLanguageKit` in HarmonyOS to extract email addresses from text. By creating a `TextProcessingEntity` component, users can input text containing email addresses, click a button to extract the email addresses, and finally display the extracted email addresses.

## Implementation Steps
1. Import the `textProcessing` module and `EntityType` from `@kit.NaturalLanguageKit`.
2. Create a `TextProcessingEntity` component, which includes a text input box, a text area to display the extracted email address, and an extraction button.
3. Implement the event handler for the button click, and call `textProcessing.getEntity` to extract entities from the text.
4. Filter the extracted entities to find the email address and display it.

## Implementation Code
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

## Conclusion
This case demonstrates the method of using `@kit.NaturalLanguageKit` for entity extraction in HarmonyOS. By creating components and using the `textProcessing.getEntity` method, we have implemented the function of extracting email addresses from text. The key knowledge points include the creation of HarmonyOS components, state management, the use of asynchronous functions, and entity extraction. This method can serve as a foundation to expand more complex natural language processing functions.