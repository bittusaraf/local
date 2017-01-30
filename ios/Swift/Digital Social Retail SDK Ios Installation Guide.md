![DSR logo](http://cloud.digitalsocialretail.com/img/logo-long-v2.png)

# Digital Social Retail SDK Ios Installation Guide
Technical support: support@digitalsocialretail.com

Last production version : 2.2.1 - 30 January 2017

## 1. Introduction

This document is destined to the iOS developer of your app and will guide him to install Digital Social Retail SDK in to your iOS app. The operation should be really simple, because you just need to add entry points into your AppDelegate.h, AppDelegate.m and info.Plist files. No other files will be modified.

Requirements: 
- Xcode 6 or later
- The SDK is compatible with iOS 7.1 or later

## Getting started

[x] Download and Unzip this file : [Download](res/Digital_Social_Retail_SDK_iOS_v2.2.1.zip)

It contains 1 file:
- **SocialRetailSRSDK.framework**: this file contains the public headers that will be used to integrate the sdk in to your application.

[x] Open your Xcode project

[x] Drag and drop SocialRetailSRSDK.framework file in embedded binaries under target of your project. In the popup, choose “Copy items if needed”
like indicated in this screenshot:
![DSR import framework](res/importFramework.png)

[x] Build settings: Go to your application Targets ->Build Settings ->Other Linker Flags and set it to: *-all_load -ObjC* like indicated in this screenshot:
![DSR build settings](res/build-settings.png)

## info.Plist settings

[x] ]Add these rows :

- **NSLocationAlwaysUsageDescription** : < put your text, You can customise the text of the field NSLocationAlwaysUsageDescription >
- **NSLocationWhenInUseUsageDescription** : < put your text, You can customise the text of the field NSLocationWhenInUseUsageDescription >
- **NSBluetoothPeripheralUsageDescription** : < put your text, You can customise the text of the field NSBluetoothPeripheralUsageDescription >
- **App Transport Security Settings > Allow Arbitrary Loads : YES**
- **Required background modes > Item 0 : App communicates using CoreBluetooth**
- **Required background modes > Item 1 : App downloads content from the network**
- **Required background modes > Item 2 : App registers for location updates**

Your info.plist should looks like:
![DSR build settings](res/info-plist.png)

[x] To import SocialRetailSRSDK header in Swift add new Objective-C file into your project xcode will automatically create bridge file like this "yourfilename-Bridging-Header.h". Like indicated in this screenshot: 
![DSR build settings](res/importHeaderSwift.png)

[x] Import the SDK header: Add this import in "yourfilename-Bridging-Header.h" file:
```Objective-C
#import <SocialRetailSRSDK/SocialRetailSRSDK.h>
```

## Modifications to AppDelegate.swift
[x] AppDelegate class start should be like this:
```Swift
import UIKit
import UserNotifications
@UIApplicationMain
class AppDelegate: UIResponder,UIApplicationDelegate,UNUserNotificationCenterDelegate{
var window: UIWindow?
var srBeaconManager:SRBeaconManager?
```
[x] Main entry point:

```Swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    srBeaconManager = SRBeaconManager();
    srBeaconManager?.startBeaconDetection();
    return true
}
```


[x] Entry point for localNotifications in the application:

```Swift
func application(_ application: UIApplication, didReceive notification: UILocalNotification) {

}
```

[x] Link the AD Web View in your application root. To do so, implement your showWebViewController method like this:

```Objective-C
-(void)showWebViewController:(SRWebViewController *)webViewController{
    if([[[[UIApplication sharedApplication]delegate]window].rootViewController isKindOfClass:[UINavigationController class]]) {
        UINavigationController *navController=(UINavigationController*)    [[[UIApplication sharedApplication]delegate]window].rootViewController;
        [navController presentViewController:webViewController animated:YES completion:nil];
}
else{
        UIViewController *navController=(UIViewController*)    [[[UIApplication sharedApplication]delegate]window].rootViewController;
        [navController presentViewController:webViewController animated:YES completion:^{

}];
    }
}
```

[x] Set application state for applicationWillResignActive:

```Swift
func applicationWillResignActive(_ application: UIApplication) {
    srBeaconManager?.willResignActive();
}
```

[x] Set application state for applicationDidBecomeActive:

```Swift
func applicationDidBecomeActive(_ application: UIApplication) {
    srBeaconManager?.didBecomeActive();
}
```

[x] Set application state for applicationWillTerminate:

```Swift
func applicationWillTerminate(_ application: UIApplication) {
    srBeaconManager?.willTerminate();
}
```

## Miscellaneous
If you get this error : *...SocialRetailSRSDK.framework/SocialRetailSRSDK(WebServiceManager.o)' does not contain bitcode*, You must rebuild it with bitcode enabled (Xcode setting ENABLE_BITCODE), obtain an updated library from the vendor, or disable bitcode for this target. for architecture arm64 clang: error: linker command failed with exit code 1 (use -v to see invocation)
Just change Enable bitcode from Yes to No


-   Congratulations, you already integrated Digital Social Retail SDK in to your application.   -

## Test your app

To do so, you need to have one Social Retail beacon with you, configured with a notification message, near your device. Make sure to have internet connection in your device and Bluetooth activated. Launch your app, accept to receive notifications and after 1-5 seconds, you should receive the notification. Then tap on ok and you should see the Ad in full screen. Tap on the button “<” in the header and the Ad should disappear.

Lock your device, wait few seconds and unlock it. Then you should see your app icon in the bottom left of the screen. You can now close the app.

If you need any support, feel free to contact our IT teams by simply sending them a email to support@digitalsocialretail.com


All the best
Digital Social Retail R&D team
