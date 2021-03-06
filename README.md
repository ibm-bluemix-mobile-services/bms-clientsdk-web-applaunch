IBM Cloud Mobile Service - AppLaunch Web Javascript Client SDK
==========================================

[![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch.svg?branch=master)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch)

This Web SDK for App Launch on IBM Cloud services, provides a library for developers to build Web applications.

>App Launch on IBM Cloud services enables the developers to build engaging apps by controlling reach and roll out of App features while measuring the defined metrics.

Ensure that you go through [IBM Cloud App Launch service documentation](https://console.bluemix.net/docs/services/app-launch/index.html) before you start.

## Build Status

| Master | Development |
|:------:|:-----------:|
|  [![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch.svg?branch=master)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch)      |    [![Build Status](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch.svg?branch=development)](https://travis-ci.org/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch)         |

## Contents
- [Setup App Launch Service](#setup-app-launch-service)
     - [Creating the service](#creating-the-service)
     - [Creating a feature](#creating-a-feature)
     - [Creating an audience](#creating-an-audience)
     - [Creating an engagement](#creating-an-engagement)
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

## Setup App Launch Service

### Creating the service
![Create feature](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_service.gif)
### Creating a feature
![Create feature](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_feature.gif)
### Creating an audience
![Create audience](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_audience.gif)
### Creating an engagement
![Create engagement](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-applaunch/blob/development/Images/create_engagement.gif)

## Installation

This section describes how to install and use the App Launch SDK for JavaScript client and to further develop your Web applications.

For installing the Javascript SDK in your web applications follow the steps -

- Download the `icapplaunch.js` and `icapplaunch-inapp.css` from [here](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-web-applaunch)

- Include the IBM Cloud AppLaunch CSS File in your web application

  `<link rel="stylesheet" type="text/css" href="icapplaunch-inapp.css">`

- Include IBM Cloud AppLaunch Web SDK to the web application.

  `<script src="icapplaunch.js" async></script>`
    
## Enabling Web applications to use IBM App Launch

### Initializing the AppLaunch SDK


##### 1. Build Configuration Object

```
var config  = new IBMAppLaunch.AppLaunchConfigBuilder().fetchPolicy(IBMAppLaunch.RefreshPolicy.REFRESH_ON_EVERY_START).cacheExpiration(30).eventFlushInterval(30).deviceID("deviceUUid").build();
```
The AppLaunchConfig builder is used to customize the following:

- `eventFlushInterval` : Sets/Decide the time interval on when the events should be sent to the server. The default value is 30 minutes.

- `cacheExpiration` : Sets/Decide the time interval until when the actions should be valid for. The default value is 30 minutes. 

	**Note** This parameter is effective only if the fetch policy is set to `IBMAppLaunch.RefreshPolicy.REFRESH_ON_EXPIRY` or `IBMAppLaunch.RefreshPolicy.BACKGROUND_REFRESH`


- `fetchPolicy` : This parameter decides on how frequently the actions should be fetched from the server. The values can be one of the following:

 	-`IBMAppLaunch.RefreshPolicy.REFRESH_ON_EVERY_START`
  
  	-`IBMAppLaunch.RefreshPolicy.REFRESH_ON_EXPIRY`
 
  	-`IBMAppLaunch.RefreshPolicy.BACKGROUND_REFRESH`
  	
- `deviceId`: This parameter must be unique. If not specified, default deviceID generation mechanism is used by SDK.
 
	**Note**: Do not rely on the default implementation of the deviceID generation  mechanism as it is not guarenteed to be unique.

##### 2. Build User Object

```
var user = new IBMAppLaunch.AppLaunchUserBuilder().userID("vittal").attributes("email","vittalpai@xyz.com").build();
```

The AppLaunchUser builder is used to provide the following information:

- `userId`: The user to be registered

- `custom`: This can be used to pass any optional custom user attributes. 

##### 3. Initialize App Launch SDK

```
IBMAppLaunch.initialize(IBMAppLaunch.ICRegion.US_SOUTH, "appGUID", "clientSecret", config, user).then(
	function(success) {
	  // Intialization Success
	}, function(failure) {
	  // Intialization Failure
	});
```


Where `region` parameter specifies the location where the app is hosted. You can use any of the following values:

- `IBMAppLaunch.ICRegion.US_SOUTH`
- `IBMAppLaunch.ICRegion.UNITED_KINGDOM`
- `IBMAppLaunch.ICRegion.SYDNEY`
- `IBMAppLaunch.ICRegion.US_SOUTH_STAGING`
- `IBMAppLaunch.ICRegion.UNITED_KINGDOM_STAGING`

The `appGUID` is the app launch app GUID value, while `clientSecret` is the appLaunch client secret value which can be obtained from the service console.

### Feature toggle

* Use the ```IBMAppLaunch.isFeatureEnabled("feature-code");``` to check if the feature is enabled for the app.

* Use the ```IBMAppLaunch.getPropertyofFeature("feature-code","property-code")``` to get the value of the particular property in a feature.

### Metrics

To send metrics to the server use the ```IBMAppLaunch.sendMetrics(["metric-code"]);``` API. This API call sends the metrics information to the server.
 
### InApp Messages

To display InApp messages invoke the following API

```
IBMAppLaunch.displayInAppMessages();
```

### Destroy

This method unregisters the user from AppLaunch Service and clears the cache

```
IBMAppLaunch.destroy().then(
   function(success){
       //Success
   }, function(failure) {
       //Failure
   });
```

### Samples and Videos

* For samples, visit - [Github Sample](https://github.com/ibm-cloud-applaunch/vroom)


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
