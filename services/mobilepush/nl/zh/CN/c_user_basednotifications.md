---

copyright:
 years: 2015, 2017

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# 启用基于用户的通知
{: #user_based_notifications}
上次更新时间：2017 年 2 月 28 日
{: .last-updated}

基于用户标识的 {{site.data.keyword.mobilepushshort}} 针对具有定制消息的移动应用程序用户。使用基于用户的通知，您可以根据首选项选择通知特定个人。

## 使用用户标识注册设备
{: #register_device_wh_userid}

要按用户标识向目标推送通知，确保您在设置了用户标识字段的情况下注册设备。     

用户标识可以是应用程序提供给设备注册 API 的任何字符串。通常，移动应用程序首先会运行认证周期，在这期间会针对认证服务（如 [{{site.data.keyword.amafull}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/docs/services/mobileaccess/index.html){: new_window}）认证移动应用程序用户。在成功认证之后，已认证的用户标识会传递至推送设备注册 API。 

要注册基于用户标识的通知，请完成以下步骤：

### Android
{: #android-register}

使用 {{site.data.keyword.mobilepushshort}} 服务的 `AppGUID` 和 `clientSecret` 键来初始化 MFPPush 类。
```
// Initialize the Push Notifications service
push = MFPPush.getInstance();
push.initialize(getApplicationContext(),"AppGUID", "clientSecret");
```
	{: codeblock}


- **AppGUID**：此为 {{site.data.keyword.mobilepushshort}} 服务的 AppGUID 键。
- **clientSecret**：此为 {{site.data.keyword.mobilepushshort}} 服务的 clientSecret 键。

  使用 **registerDeviceWithUserId** API 为设备注册 {{site.data.keyword.mobilepushshort}}。

```
// Register the device to Push Notifications
push.registerDeviceWithUserId("userId",new MFPPushResponseListener<String>() {
		@Override
		public void onSuccess(String response) {
		Log.d("Device is registered with Push Service.");}
		@Override
    public void onFailure(MFPPushException ex) {
         Log.d("Error registering with Push Service...\n"
        + "Push notifications will not be received.");
		}
		});
```
	{: codeblock}

- **userId**：传递用于注册 {{site.data.keyword.mobilepushshort}} 的唯一用户标识值。

**注：**要启用用户标识所针对的 {{site.data.keyword.mobilepushshort}}，请确保您是使用用户标识注册设备的，并且还需要传递在供应 {{site.data.keyword.mobilepushshort}} 服务时分配的“clientSecret”。没有有效的 clientSecret，设备注册将失败。

### Cordova
{: #cordova_register}

使用以下 API 来注册基于 UserId 的 {{site.data.keyword.mobilepushshort}}。

```
// Register device for Push Notification with UserId
var options = {"userId": "Your User Id value"};
BMSPush.registerDevice(options,success, failure); 
```
	{: codeblock}


- **userId**：传递用于注册 {{site.data.keyword.mobilepushshort}} 的唯一用户标识值。


### Swift
{: #swift-register}

```
// Initialize the BMSPushClient
let push =  BMSPushClient.sharedInstance
push.initializeWithAppGUID("appGUID", clientSecret:"clientSecret")
```
	{: codeblock}


- **AppGUID**：此为 {{site.data.keyword.mobilepushshort}} 服务的 AppGUID 键。
- **clientSecret**：此为 {{site.data.keyword.mobilepushshort}} 服务的 clientSecret 键。

使用 **registerWithUserId** API 为设备注册 {{site.data.keyword.mobilepushshort}}。

```
// Register the device to Push Notifications service
push.registerWithDeviceToken("deviceToken", WithUserId: "userId")  { (response, statusCode, error) -> Void in
if error.isEmpty {
  print( "Response during device registration : \(response)")
        print( "status code during device registration : \(statusCode)")
    } else {
        print( "Error during device registration \(error) ")
    }
    }
```
	{: codeblock}

- **userId**：传递用于注册 {{site.data.keyword.mobilepushshort}} 的唯一用户标识值。

### Google Chrome、Safari 和 Mozilla Firefox
{: #web-register}

使用以下 API 来注册基于用户标识的通知。使用 `app GUID`、`app Region` 和 `Client Secret` 初始化 SDK。

```
var bmsPush = new BMSPush();
var params = {
    "appGUID":"push app GUID",
    "appRegion":"App Region",
    "clientSecret":"Push Client Secret" 
    }
  bmsPush.initialize(params, function(response){
          alert(response.response)
      })
```
	{: codeblock}
  
成功初始化之后，使用用户标识注册 Web 应用程序。

```
    bmsPush.registerWithUserId("UserId", function(response) {
      alert(response.response)
  })
```
	{: codeblock}

### Google Chrome Apps and Extensions
{: #web-register-new}

使用以下 API 来注册基于用户标识的通知。使用 `app GUID`、`app Region` 和 `Client Secret` 初始化 SDK。

```
var bmsPush = new BMSPush();
var params = {
    "appGUID":"push app GUID",
    "appRegion":"App Region",
    "clientSecret":"Push Client Secret" 
    }
  bmsPush.initialize(params, function(response){
          alert(response.response)
      })
```
	{: codeblock}
  
成功初始化后，您需要使用用户标识注册 Web 应用程序。

```
    bmsPush.registerWithUserId("UserId", function(response) {
      alert(response.response)
  })
```
	{: codeblock}

## 使用基于用户标记的通知 
{: #using_userid}

基于用户标识的通知是针对特定用户的通知消息。一个用户可注册多个设备。以下步骤描述了如何发送基于用户标识的通知。

1. 从 **Push Notification** 仪表板，选择**发送通知**选项。
1. 在**发送至**选项列表中，选择**用户标识**。
1. 在**用户标识**字段中，搜索要使用的用户标识并单击 **+添加**。
![通知屏幕](images/user_notification.jpg)
1. 在**消息**字段中，输入要在通知中发送的文本。
1. 单击**发送**。


## 同步用户登录和注销 
{: #sync_login_logout}

您可以选择仅在用户登录时发送通知。 

例如，假设设备由家庭或工作团队的成员共享，而您需要定址特定用户。在此用例中，将会有用户登录和注销序列。此认证机制允许应用程序跟踪应用程序当前用户的身份。这样确保针对特定用户的通知始终由该用户接收。成功登录之后，调用传递登录用户的用户标识的设备注册 API。同样，在注销之前，调用设备注销 API。使用登录和注销对这些 Push API 进行序列化可确保针对特定用户的通知仅会发送到该用户。
