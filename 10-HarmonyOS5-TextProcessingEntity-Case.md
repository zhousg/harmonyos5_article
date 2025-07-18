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

## Application Scenarios

Entity extraction is a key capability of natural language processing services. It can comprehensively consider contextual information to accurately identify various types of entities from text:

- **Datetime (DATETIME)**: Extract specific dates from text, such as "June 18, 2024".
- **Email (EMAIL)**: Identify email addresses in text, such as "example@abc.com".
- **Express Number (EXPRESS_NO)**: Extract express tracking numbers from text.
- **Flight Number (FLIGHT_NO)**: Locate flight numbers in text, such as "CA1234".
- **Location (LOCATION)**: Extract detailed address descriptions from text.
- **Name (NAME)**: Find personal names in text.
- **Phone Number (PHONE_NO)**: Identify mobile phone numbers in text.
- **URL (URL)**: Extract website links from text.
- **Verification Code (VERIFICATION_CODE)**: Locate verification codes in text.
- **ID Number (ID_NO)**: Identify ID card numbers in text.

By accurately extracting the above - mentioned various types of entity information, this capability can be widely applied in various scenarios such as news reading, information retrieval, customer service, social chatting, and financial operations. For example, in the news reading scenario, entity extraction can be performed on the news body, and key entity information such as person names, locations, times, and URLs can be highlighted to help readers quickly grasp the main points of the article; in the customer service scenario, by extracting information such as mobile phone numbers, express tracking numbers, and verification codes from user messages, customer service representatives can more efficiently locate problems and provide solutions. Entity extraction provides solid basic support for intelligent applications in various industries.

## Constraints

- **Supported Languages**: Simplified Chinese, English, Traditional Chinese.
- **Supported Entities**: Datetime, Location, Email, Express Number, Flight Number, Name, Phone Number, URL, Verification Code, ID Number.
- **Text Length**: No more than 1000 characters.


