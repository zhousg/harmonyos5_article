# HarmonyOS 5 News Application - System Sharing Function Implementation Case

## Summary
This article details the implementation method of the system sharing function in a HarmonyOS 5.0 news application. By leveraging the `BarButton` component and the `systemShare` module, the sharing function of news links is achieved.

![Effect Diagram](img04.png)

## Implementation Steps
1. Create an instance of the `BarButton` component and set the sharing icon.
2. Add a click event to the `BarButton` component.
3. Create a `SharedData` object in the click event and set the sharing data.
4. Create an instance of `ShareController` and pass in the sharing data.
5. Obtain the UI context and call the `show` method to display the sharing interface.

## Code Implementation
```typescript
BarButton({ icon: $r('sys.media.ohos_ic_public_share') }) 
         .onClick(() => { 
           // system share 
           const data = new systemShare.SharedData({ 
             utd: uniformTypeDescriptor.UniformDataType.HYPERLINK, 
             title: 'NewsAPP', 
             content: ' `https://edition.cnn.com/` ' 
           }) 
           // Explanation
           // utd: Uniform Type Descriptor, used to specify the type of shared data, here it is a hyperlink.
           // title: Sharing title, here it is the name of the news application.
           // content: Shared content, here it is the link of the news application.
           const controller = new systemShare.ShareController(data) 
           const ctx = this.getUIContext().getHostContext() as common.UIAbilityContext 
           controller.show(ctx, { 
             previewMode: systemShare.SharePreviewMode.DETAIL, 
             selectionMode: systemShare.SelectionMode.SINGLE 
           }) 
         }) 
```

## Access Modes
- Full Access Mode
  Directly use the system sharing panel. It is suitable for Huawei - developed applications and developers who have no commercial requirements for the sharing method area. They can directly use the system panel to reduce development costs.
- Partial Access Mode
  Developers develop their own sharing capability panel and provide a system sharing entry in the sharing panel. It is suitable for developers who have commercial requirements for the sharing method area or have their own unique business logic.

## Conclusion
This article implements the system sharing function of the news application. The key knowledge points include the use of the `systemShare` module, the creation of `SharedData` objects and `ShareController` instances, and how to trigger the sharing operation in the click event. Through these steps, the system sharing function can be easily integrated into HarmonyOS applications.

## Application Scenarios
Share Kit provides applications with the ability to share content such as text, images, and videos across applications and devices.

Applications configure the content to be shared and the preview style to Share Kit, which will be used according to different scenarios: For inter - application sharing scenarios, a sharing panel is constructed based on information such as the data type and quantity of the shared content. It provides users with content previews, recommended contacts for sharing, associated applications, and operation interfaces, facilitating users to quickly select sharing applications or operations and distribute the content to the target applications. For cross - device sharing scenarios, a preview interface is constructed based on information such as the data type and quantity of the shared content for cross - device sharing.

## Constraints and Limitations
Both the host application and the target application must define data types in accordance with the UTD (Uniform Type Descriptor) specification defined by the UDMF (Uniform Data Management Framework). The target application needs to configure the supported types in the application configuration file. For example, to support all image types, it can be declared as: `general.image`.

The total amount of sharing data description information configured by the host application cannot exceed 200KB, and the total number of sharing items cannot exceed 500.
