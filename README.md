name: Flutter APK Build

# यह वर्कफ़्लो तब चलेगा जब आप कोड पुश करेंगे या मैनुअली स्टार्ट करेंगे
on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    # 1. गिटहब रिपोजिटरी से कोड लाना
    - name: Checkout repository
      uses: actions/checkout@v4

    # 2. जावा सेटअप करना (Android बिल्ड के लिए ज़रूरी है)
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        distribution: 'temurin'
        java-version: '17'

    # 3. फ्लटर सेटअप करना
    - name: Set up Flutter
      uses: subosito/flutter-action@v2
      with:
        channel: 'stable'

    # 4. ऐप की डिपेंडेंसी डाउनलोड करना
    - name: Install dependencies
      run: flutter pub get

    # 5. APK फ़ाइल बिल्ड करना
    - name: Build APK
      run: flutter build apk --release

    # 6. बने हुए APK को डाउनलोड के लिए गिटहब पर अपलोड करना
    - name: Upload APK Artifact
      uses: actions/upload-artifact@v4
      with:
        name: release-apk
        path: build/app/outputs/flutter-apk/app-release.apk
        
