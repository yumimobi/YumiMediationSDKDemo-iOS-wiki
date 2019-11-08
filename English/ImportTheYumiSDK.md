# Import the Yumi SDK
## CocoaPods (preferred)
The simplest way to import the SDK into an iOS project is to use [CocoaPods](https://guides.cocoapods.org/using/getting-started). Open your project's Podfile and add this line to your app's target:

```ruby
pod "YumiMediationSDK"
```

If you want integrate more third-party ad networks:
```ruby
pod "YumiMediationAdapters", :subspecs => ['AdColony','AdMob','AppLovin','Baidu','Chartboost','Domob','Facebook','GDT','InMobi','IronSource','Unity','Vungle','Mintegral','OneWay','ZplayAds','TapjoySDK','BytedanceAds','InneractiveAdSDK','PubNative']
```
Then run the followings at command line interface:

```shell
$ pod install --repo-update
```
Finally, open project by workspace. 

If you're new to CocoaPods, see their [official documentation](https://guides.cocoapods.org/using/using-cocoapods) for info on how to create and use Podfiles.

## Manual Download
1. Download ([SDKDownloadPage-iOS](https://github.com/yumimobi/YumiMediationSDKDemo-iOS/blob/master/normalDocuments/iOSDownloadPage.md)) YumiMediationSDK and third-party networks which you need.

2. Add YumiMediationSDK and third-party networks to your project
![addFiles](resources/addFiles.png)
![addFiles-2](resources/addFiles-2.png)

3. Script configuration

   Add YUMISDKConfig according to steps as shown.
![ios02](resources/ios02.png) 

4. Import Framework

   Import system dynamic frameworks as shown.
![ios06](resources/ios06.png) 