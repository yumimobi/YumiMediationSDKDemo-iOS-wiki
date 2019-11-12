# 开屏广告

## 初始化开屏

为了保证开屏的展示，我们推荐尽量在 App 启动时开始执行下面的方法。

例如：在您 `AppDelegate.m` 的 `application:didFinishLaunchingWithOptions:` 方法中。

```objective-c
#import <YumiMediationSDK/YumiMediationSplash.h>
@interface AppDelegate () <YumiMediationSplashAdDelegate>
@property (nonatomic) YumiMediationSplash *yumiSplash;
@end

@implementation AppDelegate    
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {  	
  self.yumiSplash = [[YumiMediationSplash alloc] initWithPlacementID:@"YOUR_PLACEMWNT_ID" 
                                                           channelID:@"YOUR_CHANNEL_ID" 
                                                           versionID:@"YOUR_VERSION_ID"];
  self.yumiSplash.delegate = self;  
  return YES;
}
@end
```

## 设置开屏拉取时长
```objective-c
/// 拉取广告超时时间，默认3s。在该超时时间内，如果广告拉取成功，则立刻展示开屏广告，否则放弃此次广告展示机会。
/// 百度 平台不支持 这个参数
[self.yumiSplash setFetchTime:3]; 
```

## 设置开屏加载时的背景图片
```objective-c
/// The background image when splash is loaded, preferably the image of the APP launch
[self.yumiSplash setLaunchImage:[UIImage imageNamed:@"YOUR_IMAGENAME"]];
```

## 设置开屏方向（只支持 Admob 平台）
```objective-c
/// splash orientation
[self.yumiSplash setInterfaceOrientation:UIInterfaceOrientationPortrait];
```

## 展示全屏广告
```objective-c
[self.yumiSplash loadAdAndShowInWindow:[UIApplication sharedApplication].keyWindow];
```

## 展示半屏广告
```objective-c
/// bottom view 的高度不能超过屏幕高度的15%
UIView *bottomView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, [UIScreen mainScreen].bounds.size.height * 0.10)];
bottomView.backgroundColor = [UIColor redColor];
[self.yumiSplash loadAdAndShowInWindow:[UIApplication sharedApplication].keyWindow withBottomView:bottomView];
```

## 实现代理方法
```objective-c
- (void)yumiMediationSplashAdSuccessToShow:(YumiMediationSplash *)splash {
  NSLog(@"yumiMediationSplashAdSuccessToShow.");
}
- (void)yumiMediationSplashAdFailToShow:(YumiMediationSplash *)splash withError:(NSError *)error {
  NSLog(@"yumiMediationSplashAdFailToShow: %@", error);
}
- (void)yumiMediationSplashAdDidClick:(YumiMediationSplash *)splash {
  NSLog(@"yumiMediationSplashAdDidClick.");
}
- (void)yumiMediationSplashAdDidClose:(YumiMediationSplash *)splash {
  NSLog(@"yumiMediationSplashAdDidClose.");
}
```