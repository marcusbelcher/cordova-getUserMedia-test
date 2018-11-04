# Cordova getUserMedia example
Below are the steps I made in order to allow WebRTC getUserMedia access to a camera inside of a Cordova application. Working builds on Browser and Android only.

## Set up and tools
- Visual Studio Code 1.28.2
- Android Studio 3.2.1
- Build #AI-181.5540.7.32.5056338, built on October 9, 2018
- JRE: 1.8.0_152-release-1136-b06 amd64
- JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
- Windows 10 10.0
- Git
- 8.1.2 (cordova-lib@8.1.1)

## Known issues
### iOS
- Currently Apple has banned getUserMedia access inside of the WKWebView which Cordova uses as the app browser (this also applies to UIWebView). Outside of the native Safari application no getUserMedia access is allowed until Apple rectifies this. An educated gestimate is iOS 13 in ~September 2019...

### Android
- API level 21 is a must for this to function. 
- onPermissionRequest needs to be overrided on the Chromium webview or permissions management within the application (see Cordova plugin)

### All platforms
- Serving any getUserMedia code inside of an iframe will fail security permissions. To get around this you need to use the allow="camera" policy. See HTTPS://developer.mozilla.org/en-US/docs/Web/HTTP/Feature_Policy

## Steps
### 1. Set up working Browser example
- npm install -g cordova 
- cordova create getUserMediaApp
- cd getUserMediaApp
- cordova platform add browser
- Copy and paste code from HTTPS://github.com/marcusbelcher/wasm-asm-camera-webgl-test into `./www/index.html` and `./www/js/index.js`
- Minor refactoring of code to suite Cordova
- cordova run browser
- Captured screenshot and saved as browser_success.jpg
- In Chrome manually blocking the camera and captured a screenshot saved as browser_error.jpg (highlighted in Yellow is the camera options and the icon shows as blocked)

### 2. Set up failing Android example
The aim here is to get a working failing example. We want to have the Browser example but with a getUserMedia error. This is to test and make sure your Android set up is correct. Make sure you go through the Android instructions and that you have installed the right Android Studio, etc. Please see this first: HTTPS://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html

- cordova platform add android
- Captured screenshot and saved as android_error.PNG

### 3. Apply bugfixes to Android example
- Add `<uses-permission android:name="android.permission.CAMERA" /><uses-feature android:name="android.hardware.camera" /><uses-feature android:name="android.hardware.camera.autofocus" /><uses-feature android:name="android.hardware.camera2.full" /><uses-feature android:name="android.hardware.camera2.autofocus" /><uses-permission android:name="android.permission.INTERNET" /><uses-permission android:name="android.webkit.PermissionRequest" />` to `./platforms/android/app/src/AndroidManifiest.xml`
- cordova plugin add cordova-plugin-android-permissions
- Inside if `./www/js/index.js` utilise the cordova.plugins.permissions plugin and query for the Camera. On success run the test code.
- Captured screenshot and saved as android_success.PNG

### 4. Set up failing iOS example
The aim here is to get a working failing example. We want to have the Browser example but with a getUserMedia error. This is to test and make sure your iOS set up is correct. Make sure you go through the iOS instructions and that you have installed the right XCode, etc. Please see this first: HTTPS://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html
- TODO

### 5. Apply bugfixes to iOS example
Currently not technically possible due to WKWebView permission restrictions set by Apple. As soon as this is possible I will update this example.