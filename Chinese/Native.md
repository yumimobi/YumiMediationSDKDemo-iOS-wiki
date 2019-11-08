# 原生广告

## 初始化及请求
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

## 何时请求广告
展示原生广告的应用可以在实际展示广告之前随时请求这些广告。在许多情况下，这是推荐的做法。例如，如果某款应用展示一个商品清单，其中会夹杂一些原生广告，那么该应用就可以加载整个清单中的原生广告，因为它知道一些广告仅在用户滚动浏览视图后才会展示，还有一些可能根本不会展示。

  - 注意：尽管预先提取广告是一种很好的方法，但务必不要长久保留旧广告而不进行展示。对任何原生广告对象来说，如果在保留30分钟后仍没有获得展示，就应该予以舍弃，并替换为来自新请求的新广告。
  您可通过 `YumiMediationNativeModel.h` 中的 `-(BOOL)isExpired;` 来判断当前广告是否过期。

  - 注意：重复使用 `loadAd:` 时，请确保等待每个请求完成，然后再重新调用 `loadAd:`。
  - 注意：如果你要支持原生模板广告，请务必设置 `YumiMediationNativeAdConfiguration` 中的 `expressAdSize` 为你需要的模板尺寸

## Register View

```objective-c
/**
  注册用来渲染广告的 View
  - Parameter view: 渲染广告的 View.
  - Parameter clickableAssetViews: 可用于点击 view 的字典，使用 YumiMediationUnifiedNativeAssetIdentifier 为 key
  - Parameter viewController: 将用于当前的ui SKStoreProductViewController(iTunes商店产品信息)或	应用程序的浏览器。
  - Parameter nativeAd: 用于展示的原生广告模型对象
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
- 注意: 如果您使用 `UIButton` 来展示原生广告元素，必须禁用 `userInteractionEnabled`，以便 SDK 处理事件。
       最好的方式是避免使用 `UIButton`，而使用 `UILabel` 或者 `UIImageView`。

- 如果以这种方式注册视图， SDK 就可以自动处理诸如以下任务：
  1. 记录点击
  2. 显示广告选择叠加层 （AdMob，Facebook 支持）
  3. 显示广告标识
      
## Report Impression

```objective-c
/**
  当原生广告被展示时调用此方法
  - Parameter nativeAd: 将要被展示的广告对象.
  - Parameter view: 用来渲染广告的 View.
*/
- (void)reportImpression:(YumiMediationNativeModel *)nativeAd view:(UIView *)view;
```

## 原生视频广告

- 如果您想在原生广告中展示视频，您仅需要在注册视图时，将您的 MediaView 传入 SDK。 `YumiMediationUnifiedNativeMediaViewAsset : self.nativeView.mediaView`。
  SDK 会自动处理此填充事宜：
    1. *如果有视频素材资源可用，则系统会对其进行缓冲，并在您传入的 MediaView 中播放。*
    2. *如果广告中不包含视频素材资源，则会改为下载第一个图片素材资源，并放置在您传入的 MediaView 中。*
  
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
- 通过 `YumiMediationNativeModel.h` 中的 `hasVideoContent` 您可以判断该原生对象中是否存在视频资源。

```objective-c
/// Indicates whether the ad has video content.
@property (nonatomic, assign, readonly) BOOL hasVideoContent;
``` 

- YumiMediationNativeVideoController 提供以下查询视频状态的方法：

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

## 原生模板广告
您可通过 `YumiMediationNativeModel.h` 中的 `isExpressAdView` 来判断当前广告是否是模板广告。

如果是原生模板广告，你只需要把 `YumiMediationNativeModel.h` 中的 `expressAdView` 添加到您的广告容器中。

因为原生模板广告需要注册渲染时间，请在 `yumiMediationNativeExpressAdRenderSuccess:` 渲染成功回调中去展示您的原生模板广告。  

## 原生广告选项 `YumiMediationNativeAdConfiguration`
`YumiMediationNativeAdConfiguration` 是 `YumiMediationNativeAd` 的创建过程中包含的最后一个参数，本节将介绍这些选项。

- `disableImageLoading` 

  通过包含 `image`, `imageURL` 和 `ratios` 属性的 YumiMediationNativeAdImage 实例返回原生广告的图片素材资源。如果 disableImageLoading 设置为 false（这是默认值，在 Objective-C 中为 NO），则 SDK 会自动获取图片素材资源，并为您填充各项属性。不过，如果设置为 true（在 Objective-C 中为 YES），SDK 将只填充 imageURL，从而允许您自行决定是否下载实际图片。
  
- `preferredAdChoicesPosition`

  您可以使用该属性指定“广告选择”图标应放置的位置。该图标可以显示在广告的任一角，默认为 YumiMediationAdChoicesPositionTopRightCorner。
  
- `preferredAdAttributionPosition` 

  您可以使用该属性指定广告标识图标应放置的位置。该图标可以显示在广告的任一角，默认为 YumiMediationAdViewPositionTopLeftCorner。
  
- `preferredAdAttributionText`

  您可以使用该属性指定广告标识的文案。根据手机语言显示为“广告”或者“Ad”。
  
- `preferredAdAttributionTextColor`

  您可以使用该属性指定广告标识的文字颜色。默认白色。

- `preferredAdAttributionTextBackgroundColor` 

  您可以使用该属性指定广告标识的背景颜色。默认灰色。

- `preferredAdAttributionTextFont`

  您可以使用该属性指定广告标识的字体大小。默认10。
  
- `hideAdAttribution`

  您可以使用该属性指定广告标识是否显示。默认显示。
  
- `expressAdSize`

  您可以使用该属性指定请求原生模板广告的大小。默认为nil

## 实现代理方法

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

