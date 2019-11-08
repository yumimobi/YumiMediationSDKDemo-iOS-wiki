# GDPR
本文件是为遵守欧洲联盟的一般数据保护条例(GDPR)而提供的。
自 YumiMediationSDK 4.1.0 起，如果您正在收集用户的信息，您可以使用下面提供的 api 将此信息通知给 YumiMediationSDK 和部分三方平台。
更多信息请查看我们的官网。

## 设置 GDPR

```objective-c
typedef enum : NSUInteger {
    /// The user has granted consent for personalized ads.
    YumiMediationConsentStatusPersonalized,
    /// The user has granted consent for non-personalized ads.
    YumiMediationConsentStatusNonPersonalized,
    /// The user has neither granted nor declined consent for personalized or non-personalized ads.
    YumiMediationConsentStatusUnknown,
} YumiMediationConsentStatus;
```

```objective-c
// Your user's consent. In this case, the user has given consent to store and process personal information.
[[YumiMediationGDPRManager sharedGDPRManager] updateNetworksConsentStatus:YumiMediationConsentStatusPersonalized];
```
### 支持 GDPR 的平台
统计自 YumiMediationSDK 4.1.0 起。
详细信息请至各平台官网获取。

| 平台名称 | 是否支持 GDPR | 备注 |
| :----: | :--------:| :--: |
| Unity  | 是 |   |
| Admob  | 是 |   |
| Mintegral | 是 |   |
| Adcolony  | 是 |   |
| IronSource  | 是 |   |
| Inneractive | 是 |   |
| Chartboost | 是 |   |
| InMobi | 是 |   |
| IQzone | 是 |   |
| Yumi | 是 |   |
| AppLovin  | 是 |   |
| Baidu  | 否 |   |
| Facebook | 是 | |
| Domob  | 否 |   |
| GDT | 否 |   |
| Vungle | 否 | 可在 Vungle 后台设置 |
| OneWay | 否 |   |
| BytedanceAds | 否 |   |
| ZplayAds  | 否 |   |