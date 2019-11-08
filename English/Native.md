# Native 

## Create and Load Yumi Native Ads
```objective-c
#import <YumiMediationSDK/YumiMediationNativeAd.h>
#import <YumiMediationSDK/YumiMediationNativeAdConfiguration.h>

@interface ViewController ()<YumiMediationNativeAdDelegate>
@property (nonatomic) YumiMediationNativeAd *yumiNativeAd;
@end
 
@implementation ViewController
- (void)viewDidLoad {
  [super viewDidLoad];
  YumiMediationNativeAdConfiguration *config = [[YumiMediationNativeAdConfiguration alloc] init];
  self.yumiNativeAd = [[YumiMediationNativeAd alloc] initWithPlacementID:@"Your Placement ID"
                                                               channelID:@"Your Channel ID"                         versionID:@"Your Version ID"
                                                           configuration:config];
  self.yumiNativeAd.delegate = self;
  //You can request more than one ad.
  [self.nativeAd loadAd:1];
}
@end
```

## When to Request Ad
Apps displaying native ads are free to request them in advance of when they'll actually be displayed. In many cases, this is the recommended practice. An app displaying a list of items with native ads mixed in, for example, can load native ads for the whole list, knowing that some will be shown only after the user scrolls the view and some may not be displayed at all.

- Warning: while prefetching ads is a great technique, it's important that you don't keep old ads around forever without displaying them. Any native ad objects that have been held without display for longer than an hour should be discarded and replaced with new ads from a new request.
You can call `-(BOOL)isExpired;` from `YumiMediationNativeModel.h` file to determine whether the current AD is expired.
- Warning: when reusing `loadAd:`, make sure you wait for each request to complete.
- Warning: You must set native express ad size if you want to support native express ad. you can find `expressAdSize` property in the `YumiMediationNativeAdConfiguration.h` file.

## Register View

```objective-c
/**
  This is a method to associate a YumiNativeAd with the UIView you will use to display the native ads.
  - Parameter view: The UIView you created to render all the native ads data elements.
  - Parameter clickableAssetViews: Dictionary of asset views that are clickable, keyed by asset IDs.
  - Parameter viewController: The UIViewController that will be used to present SKStoreProductViewController(iTunes Store product information) or the in-app browser. If nil is passed, the top view controller currently shown will be used.
  - Parameter nativeAd: The current native ad model.
*/
- (void)registerViewForInteraction:(UIView *)view
               clickableAssetViews:(NSDictionary<YumiMediationUnifiedNativeAssetIdentifier, UIView *> *)clickableAssetViews
                withViewController:(nullable UIViewController *)viewController
                          nativeAd:(YumiMediationNativeModel *)nativeAd;

  /// for example:
  [self.nativeAd registerViewForInteraction:self.nativeView.adView
                        clickableAssetViews:@{
                                              YumiMediationUnifiedNativeTitleAsset : self.nativeView.title,
                                              YumiMediationUnifiedNativeDescAsset : self.nativeView.desc,
                                              YumiMediationUnifiedNativeCoverImageAsset : self.nativeView.coverImage,
                                              YumiMediationUnifiedNativeMediaViewAsset : self.nativeView.mediaView,
                                              YumiMediationUnifiedNativeIconAsset : self.nativeView.icon,
                                              YumiMediationUnifiedNativeCallToActionAsset : self.nativeView.callToAction
                                              }
                         withViewController:self
                                   nativeAd:adData];
```
- Warning: If you use UIButtons to display native ad assets, you need to disable their user interaction so that the SDK can properly receive and process UI events. Because of this extra step, it's frequently best to avoid UIButtons entirely and use UILabel and UIImageView instead.
- If you register the view in this way, the SDK automatically handles tasks such as the following:

  - handle click event.
  - display adChoice view (admob,facebook support)
  - display the `Ad` text view
      
## Report Impression

```objective-c
/**
  report impression when display the native ad.
  - Parameter nativeAd: the ad you want to display.
  - Parameter view: view you display the ad.
*/
- (void)reportImpression:(YumiMediationNativeModel *)nativeAd view:(UIView *)view;
```

## Native Video Ads

- If you want to display video in native ads, you only need to pass your MediaView into the SDK when you register the view. `YumiMediationUnifiedNativeMediaViewAsset: self. NativeView.MediaView`. The SDK will automatically handle this filling:

  - If a video resource is available, the sdk cached and played in the MediaView that you pass in.
  - If the advertisement does not contain a video source, it downloads the first image source instead and places it in the MediaView that you pass in.
  
```objective-c
[self.nativeAd registerViewForInteraction:self.nativeView.adView
                      clickableAssetViews:@{
                          YumiMediationUnifiedNativeTitleAsset : self.nativeView.title,
                          YumiMediationUnifiedNativeDescAsset : self.nativeView.desc,
                          YumiMediationUnifiedNativeCoverImageAsset : self.nativeView.coverImage,
                          YumiMediationUnifiedNativeMediaViewAsset : self.nativeView.mediaView,YumiMediationUnifiedNativeIconAsset : self.nativeView.icon,
                          YumiMediationUnifiedNativeCallToActionAsset : self.nativeView.callToAction
                          }
                       withViewController:self
                                 nativeAd:adData];
``` 
- You can call `hasVideoContent ` from `YumiMediationNativeModel.h ` to determine there contains video resources in the native model.

```objective-c
/// Indicates whether the ad has video content.
@property (nonatomic, assign, readonly) BOOL hasVideoContent;
``` 

- You can query the status of video in the following ways, from `YumiMediationNativeVideoController`

```objective-c
/// Delegate for receiving video notifications.
@property(nonatomic, weak) id<YumiMediationNativeVideoControllerDelegate> delegate;
/// Play the video. Doesn't do anything if the video is already playing.
- (void)play;
/// Pause the video. Doesn't do anything if the video is already paused.
- (void)pause;
/// Returns the video's aspect ratio (width/height) or 0 if no video is present.
/// baidu always return 0
- (double)aspectRatio;
@protocol YumiMediationNativeVideoControllerDelegate<NSObject>
@optional
/// Tells the delegate that the video controller has began or resumed playing a video.
- (void)yumiMediationNativeVideoControllerDidPlayVideo:(YumiMediationNativeVideoController *)videoController;
/// Tells the delegate that the video controller has paused video.
- (void)yumiMediationNativeVideoControllerDidPauseVideo:(YumiMediationNativeVideoController *)videoController;
/// Tells the delegate that the video controller's video playback has ended.
- (void)yumiMediationNativeVideoControllerDidEndVideoPlayback:(YumiMediationNativeVideoController *)videoController;
@end
``` 

## Native Express Ads
You can use the `isExpressAdView` in `YumiMediationNativeModel.h` to determine if the current ad is the native express ad.

If it's a native express ad, you just need to add `expressAdView` from  `YumiMediationNativeModel.h` to your express ad super view.
  
Because native express ads need to be registered for rendering time, please show your native express ads in `yumiMediationNativeExpressAdRenderSuccess:` callback.
  

## YumiMediationNativeAdConfiguration
YumiMediationNativeAdConfiguration is the last parameter when init YumiMediationNativeAd.
- `disableImageLoading` 

  Image assets for native ads are returned via instances of `YumiMediationNativeAdImage`, which contains `image`,`ratios` and `imageURL` properties. If disableImageLoading is set to false, which is the default (NO in Objective-C), the SDK will fetch image assets automatically and populate both the image and the imageURL properties for you. If it's set to true (or YES in Objective-C), the SDK will only populate imageURL, allowing you to download the actual images at your discretion.
  
- `preferredAdChoicesPosition`

  You can use this property to specify where the AdChoicesView should be placed. Default is `YumiMediationAdChoicesPositionTopRightCorner`
  
- `preferredAdAttributionPosition` 

  You can use this property to specify where the Ad text view should be placed. Default is `YumiMediationAdViewPositionTopLeftCorner`
  
- `preferredAdAttributionText`

  You can use this property to specify the Ad text. Default is `Ad`.
  
- `preferredAdAttributionTextColor`

  You can use this property to specify the Ad text color. Default is `white`.

- `preferredAdAttributionTextBackgroundColor` 

  You can use this property to specify the Ad text background color. Default is `gray`.

- `preferredAdAttributionTextFont`

  You can use this property to specify the Ad text font size. Default is `10`.
  
- `hideAdAttribution`

  You can use this property to hide the Ad text. Default is display.
  
- `expressAdSize`

  You can use this property to set express ad size. Default is nil.

## Delegate Implementation

```objective-c
/// Tells the delegate that an ad has been successfully loaded.
- (void)yumiMediationNativeAdDidLoad:(NSArray<YumiMediationNativeModel *> *)nativeAdArray{
  NSLog(@"Native Ad Did Load.");
}

/// Tells the delegate that a request failed.
- (void)yumiMediationNativeAd:(YumiMediationNativeAd *)nativeAd didFailWithError:(YumiMediationError *)error{
  NSLog(@"NativeAd Did Fail With Error.");
}

/// Tells the delegate that the Native view has been clicked.
- (void)yumiMediationNativeAdDidClick:(YumiMediationNativeModel *)nativeAd{
  NSLog(@"Native Ad Did Click.");
}

///Tells the delegate that the Native express view has been successfully rendered.
- (void)yumiMediationNativeExpressAdRenderSuccess:(YumiMediationNativeModel *)nativeModel{
  NSLog(@"Native express Ad did render success.");
}

///Tells the delegate that the Native express view render failed
- (void)yumiMediationNativeExpressAd:(YumiMediationNativeModel *)nativeModel didRenderFail:(NSString *)errorMsg{
  NSLog(@"Native express Ad did render fail.");
}

///Tells the delegate that the Native express view has been closed
- (void)yumiMediationNativeExpressAdDidClickCloseButton:(YumiMediationNativeModel *)nativeModel{
  NSLog(@"Native express Ad did click close button.");      
}
```

