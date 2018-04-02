IBM Cloud Mobile Service - AppLaunch Web Client SDK
==========================================
[![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch.svg?branch=master)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch)
[![JitPack](https://jitpack.io/v/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch.svg)](https://jitpack.io/#ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch)


This Web SDK for App Launch on IBM Cloud services, provides a library for developers to build Web applications.

>App Launch on IBM Cloud services enables the developers to build engaging apps by controlling reach and roll out of App features while measuring the defined metrics.

Ensure that you go through [IBM Cloud App Launch service documentation](https://console.bluemix.net/docs/services/app-launch/index.html) before you start.

## Build Status

| Master | Development |
|:------:|:-----------:|
|  [![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch.svg?branch=master)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch)      |    [![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch.svg?branch=development)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch)         |

## Contents
- [Setup App Launch Service](#setup-app-launch-service)
     - [Creating the service](#creating-the-service)
     - [Creating a feature](#creating-a-feature)
     - [Creating an audience](#creating-an-audience)
     - [Creating an engagement](#creating-an-engagement)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Enabling Web applications to use IBM App Launch](#enabling-android-applications-to-use-ibm-app-launch)
    - [Import the App launch SDK in your code](#import-the-app-launch-sdk-in-your-code)
    - [Initializing the AppLaunch SDK](#initializing-the-appLaunch-sdk)
- [Feature Toggle](#feature-toggle)
    - [Check if feature is enabled](#feature-toggle)
    - [Get variable for feature](#feature-toggle)
- [Metrics](#metrics)
    - [Send Metrics](#send-metrics)
- [InApp Messages](#inappmessages)
    - [Display InApp Messages](#inappmessages) 
- [Destroy](#destroy)
- [Samples and videos](#samples-and-videos)

##Setup App Launch Service

### Creating the service
![Create feature](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_service.gif)
### Creating a feature
![Create feature](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_feature.gif)
### Creating an audience
![Create audience](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_audience.gif)
### Creating an engagement
![Create engagement](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_engagement.gif)

## Prerequisites

 * Safari, Chrome or Firefox web browser
 * [Sample App](https://developer.android.com/studio/index.html)



## Installation

This section describes how to install and use the client JavaScript App Launch SDK to further develop your Web applications.

The Web SDKs for IBM Cloud Mobile services is available via [JitPack](https://jitpack.io).

### JitPack

To install App Launch Web SDK using jitpack

1. In the projects `build.gradle` file include:
   **Is this step required**

	```
	allprojects {
	    repositories {
		jcenter()
		maven { url 'https://jitpack.io' }
	    }
	}
	```

2. Add IBM Cloud App Launch Web SDK dependency and BMS Core dependency to your app module `build.gradle` file.
    
    ```
    Code to be put here 
	
    ```
3. Add the following permissions inside application's `AndroidManifest.xml` file. **Should it be `Web.xml`.

     ```
     <uses-permission android:name="android.permission.INTERNET"/>
     <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
     ```   
    
## Enabling Web applications to use IBM App Launch

### Initializing the AppLaunch SDK
A common place to put the initialization code is the`onCreate()` method of your Android application:  **change Required**

##### 1. Build Configuration Object
```
AppLaunchConfig appLaunchConfig = new AppLaunchConfig.Builder().eventFlushInterval(10).cacheExpiration(10).fetchPolicy(RefreshPolicy.REFRESH_ON_EVERY_START).deviceId("454dfidif0e-3r3434-3434343").build();
```
The AppLaunchConfig builder is used to customize the following:

- `eventFlushInterval` : Sets/Decide the time interval on when the events should be sent to the server. The default value is 30 minutes.

- `cacheExpiration` : Sets/Decide the time interval until when the actions should be valid for. The default value is 30 minutes. 

	**Note** This parameter is effective only if the fetch policy is set to `RefreshPolicy.REFRESH_ON_EXPIRY` or `RefreshPolicy.BACKGROUND_REFRESH`


- `fetchPolicy` : This parameter decides on how frequently the actions should be fetched from the server. The values can be one of the following:

 	-`RefreshPolicy.REFRESH_ON_EVERY_START`
  
  	-`RefreshPolicy.REFRESH_ON_EXPIRY`
 
  	-`RefreshPolicy.BACKGROUND_REFRESH`
  	
  	To enable BACKGROUND_REFRESH include the following in the applications AndroidManifest.xml. 
     <receiver
            android:name="com.ibm.mobile.applaunch.android.background.AppLaunchAlarmReceiver"
            android:process=":remote" >
    </receiver>

        <service
            android:name="com.ibm.mobile.applaunch.android.background.AppLaunchBackgroundService"
         android:exported="false"/>

	The default value is `RefreshPolicy.REFRESH_ON_EVERY_START`.
  	
- `deviceId`: This parameter must be unique. If not specified, default deviceID generation mechanism is used by SDK.
 
	**Note**: Do not rely on the default implementation of the deviceID generation  mechanism as it is not guarenteed to be unique.

##### 2. Build User Object

```
AppLaunchUser appLaunchUser = new AppLaunchUser.Builder().userId("norton").custom("type","premium").build();
```

The AppLaunchUser builder is used to provide the following information:

- `userId`: The user to be registered

- `custom`: This can be used to pass any optional custom user attributes. 

##### 3. Initialize App Launch SDK

```
AppLaunch.getInstance().initialize(getApplication(), "bluemixRegionSuffix","appGUID","clientSecret",appLaunchConfig,appLaunchUser,AppLaunchListener);
```

Where `region` parameter specifies the location where the app is hosted. You can use any of the following values:

- `ICRegion.US_SOUTH`
- `ICRegion.UNITED_KINGDOM`
- `ICRegion.SYDNEY`
- `ICRegion.US_SOUTH_STAGING`
- `ICRegion.UNITED_KINGDOM_STAGING`

The `appGUID` is the app launch app GUID value, while `clientSecret` is the appLaunch client secret value which can be obtained from the service console.
     
`AppLaunchListener` is the call back listener which will be used to notify in case of success and failure events. Pass the instance of the class implementing the AppLaunchListener interface.

### Feature toggle

* Use the ```AppLaunch.sharedInstance.isFeatureEnabled(featureCode: "feature code")``` to check if the feature is enabled for the app.

* Use the ```AppLaunch.sharedInstance.getPropertyofFeature(featureCode: "feature code", propertyCode: "property code")``` to get the value of the particular property in a feature.


 **Note** :The above two APIs throws `AppLaunchException` exception if `isFeatureEnabled` or `getPropertyofFeature` is invoked before `init` API.  

### Metrics

To send metrics to the server use the ```AppLaunch.sharedInstance.sendMetrics()``` API. This API call sends the metrics information to the server.

```
 AppLaunch.getInstance().sendMetrics(ArrayList<String> metrics);
```

 **Note** : The above API throws `AppLaunchException` error if `sendMetrics` is invoked before `init` API.
 
 
### InApp Messages
To display InApp messages invoke the following api

```
 AppLaunch.getInstance().displayInAppMessages(context);
```

**Note** : Here the context is the Activities context.

### Destroy

This method unregisters the user from AppLaunch Service and clears the cache

```
AppLaunch.getInstance().destroy(AppLaunchListener)
```

### Samples and Videos

* For samples, visit - [Github Sample](https://github.com/ibm-cloud-applaunch/sample-android-poll)


### Learn More

* Visit the **[IBM Cloud Developers Community](https://developer.ibm.com/bluemix/)**.

### Connect with IBM Cloud

[Twitter](https://twitter.com/ibmbluemix)|
[YouTube](https://www.youtube.com/watch?v=dQ1WcY_Ill4) |
[Blog](https://developer.ibm.com/bluemix/blog/) |
[Facebook](https://www.facebook.com/ibmbluemix) |
[Meetup](http://www.meetup.com/bluemix/)

=======================
Copyright 2016-17 IBM Corp.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
