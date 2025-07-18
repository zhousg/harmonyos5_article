// ... existing code ...

## Application Scenarios
Speech Kit (Scenario-based Voice Service) integrates voice AI capabilities, including the TextReader and AICaptionComponent. These capabilities facilitate user-device interaction and enable text reading for users. The TextReader has a wide range of applications. For example, when users are unable or inconvenient to read on - screen text, it can read news aloud to provide information. The AICaptionComponent is also widely used. For instance, it can provide caption services when users are unfamiliar with the audio source language or when the audio is muted.

## Constraints
- Supported languages: Chinese and English.
- Supported audio streams: Currently only "pcm" encoding is supported.
- Audio sampling rate: Currently only 16000 Hz is supported.
- Audio channels: Currently only 1 channel is supported.
- Audio sampling bytes: Currently only 16 - bit is supported.
- Some models are not supported yet, including nova 12, nova 12 Pro, nova 13, nova 13 Pro, and nova 14.
- Device restrictions: This Kit is only applicable to Phone, Tablet, and 2 - in - 1 devices.
- Regional restrictions: This Kit only supports services within China.