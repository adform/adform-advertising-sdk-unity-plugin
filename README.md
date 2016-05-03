# Adform Advertising SDK Unity Plugin

Adform Advertising SDK Unity Plugin provides a way to integrate Adform ads in Unity applications easily. This plugin applies for Android and IOS unity applications.

## Downloading the Plugin

Our plugin is available on [GitHub](https://github.com/adform/adform-advertising-sdk-unity-plugin).
 
To add the downloaded plugin:

1. Open your Unity project.
2. Go to `Assets -> Import Package -> Custom Package` and select the
`AdformAdvertisingSDK.unitypackage` to import.
3. In your GameObject, click `Add Component -> New Script`.
4. Integrate ads in that script. The examples could be found in `Assets/Adform/Advertising/Sample/` directory.

You will also need to add Adform Advertising SDK to your Unity project, you can find it here: [iOS](https://github.com/adform/adform-ios-sdk), [Android](https://github.com/adform/adform-android-sdk).
To add Adform Advertisign SDK to your project:

1. Open your Unity project.
2. Go to `Assets -> Import New Asset` and select the `AdformAdvertising.framework` or `advertising-sdk-xxx.aar` respectively for iOS or Android to import.

## Configuration for Android

The `Assets/Plugins/Android` folder includes a working example of `AndroidManifest.xml`. This example contains the necessary components, simply modify it to suit your needs. 

If you already have `AndroidManifest.xml` file in you android directory, modify it according to [Adform Advertising SDK documentation](https://github.com/adform/adform-android-sdk/wiki/Getting-Started#5-update-androidmanifestxml).


Adform Advertising SDK Unity Plugin also requires the [Google Play Services](http://developer.android.com/google/play-services/setup.html). Google Play Services are located at `{ANDROID_SDK_LOCATION}/extras/google/google_play_services/libproject`, please import `google-play-services_lib` folder into `Assets/Plugins/Android` directory.

## Configuration for iOS

You must make sure that these frameworks are included to unity generated xCode project for the sdk to work properly:

* UIKit.framework
* CoreGraphics.framework
* QuartzCore.framework
* AdSupport.framework
* EventKit.framework
* EventKitUI.framework
* MediaPlayer.framework
* CoreTelephony.framework
* SystemConfiguration.framework
* CoreLocation.framework
* CoreMedia.framework
* AVFoundation.framework
* AssetsLibrary.framework


## AdInline integration

### Create AdInline

Create AdInline object using `AdformAdvertisingSDK` class static method `createAdInline(masterTag)`, for example:

```c#
	IAdInline adInline = AdformAdvertisingSDK.createAdInline (12345L);
```
	
### Basic Setup

For android you have to call `onResume`, `onPause` and `onDestroy` lifecycle methods. It is mandatory for Android applications.

```c#
	void OnApplicationPause (bool pauseStatus) 
	{
		if (adInline != null) {
			if (pauseStatus) {
				adInline.onPause ();
			} else {
				adInline.onResume ();
			}
		}
	}
	
	void OnDestroy() 
	{
		if (adInline != null) {
			adInline.onDestroy ();
		}
	}
```

### Show AdInline

It is possible to show `AdInline` by relative position or by x,y coordinates. 

In order to show ad in particular position use `AdInline` method `showAtPosition (adPosition)`, for example:

```c#
	adInline.showAtPosition (AdPosition.BOTTOM_RIGHT);
```
	
To display ad by x,y coordinates use `showAbsolute(x,y)` method, for example:

```c#
	adInline.showAbsolute (30, 30);	
```
	
### Set AdInline parameters

#### Ad Size

To set the ad size you can use

```c#
	adInline.setAdSize (new AdSize(320, 50));
```
	
Smart size allows to display ads that fill the width of the screen. You could set smart size in this way:
	
```c#
	adInline.setAdSize (AdSize.SmartAdSize());
```

If you want to support multiple ad sizes at the same placement, you have to use additional dimensions:

```c#
	adInline.additionalDimensionsEnabled = true;
```
	
 If you wish to define which ad sizes can be loaded in the placement, you have to use supported dimensios:

```c#
	adInline.supportedDimensions = new List<AdSize>(){ new AdSize (320, 250), new AdSize (350, 350) };
```
		
#### Controlling animations

AdInline state changes can be animated using a set of different animations.
It is possible to define how ad is shown/hidden/updated and how ad expands to fullscreen.
This can be achieved using 'setAdTransitionStyle' and 'setModalPresentationStyle' methods.
At the moment SDK supports these animation types:

    AnimationStyle.SLIDE - When view is presented, it slides up from the bottom.
    AnimationStyle.FADE - When view is presented, it fades in.
    AnimationStyle.NONE - Presentation is not animated. You can use this animation style if you want to disable animations.

By default, inline ads use the AnimationStyle.SLIDE animation style.
	
#### Controlling dim overlay in expand state

It is possible to enable/disable application content dimming for AdInline expanded state by using 'setDimOverlayEnabled' method.

#### Controlling video settings

In order to change video settings, create `VideoSettings` object and modify needful options.

By default, when video is loaded and started, video is muted. To edit this behavior you can use `initialyMuted` property.

By default, video banner will close after its finished playing content. To edit this behavior you can use `autoClose` property.

By default, close button is always hidden when video ad is displayed. This property can be changed by defining type in `closeBehaviour` property.

Video player also has different skin types that can be set by using `controlsStyle` property.

In order to set up video ads fallback, you need to set fallback master tag(`fallbackMasterTagId` property) of other HTML ad.

Video settings example:

```c#
	VideoSettings videoSettings = new VideoSettings ();
	videoSettings.autoClose = true;
	videoSettings.closeBehaviour = VideoAdCloseBehaviour.ALLWAYS;
	videoSettings.controlsStyle = VideoPlayerControlsStyle.MINIMAL;
	videoSettings.fallbackMasterTagId = 45;
	videoSettings.initialyMuted = true;
	adInline.videoSettings = videoSettings;
```

## AdHesion integration

### Create AdHesion

Create AdHesion object using `AdformAdvertisingSDK` class static method `createAdHesion(masterTag, position)`, for example:

```c#
	IAdHesion adHesion = AdformAdvertisingSDK.createAdHesion (12345L, AdHesionPosition.BOTTOM);
```
	
### Basic Setup

For android you have to call `onResume`, `onPause` and `onDestroy` lifecycle methods. It is mandatory for Android applications.

```c#
	void OnApplicationPause (bool pauseStatus) 
	{
		if (adHesion != null) {
			if (pauseStatus) {
				adHesion.onPause ();
			} else {
				adHesion.onResume ();
			}
		}
	}
	
	void OnDestroy() 
	{
		if (adHesion != null) {
			adHesion.onDestroy ();
		}
	}
```
	
### Set AdHesion parameters

AdHesion placements are very similar to AdInline placements. Therefore, AdHesion ads 
have all the parameters that AdInline ads have (for instructions how to set them check out 
[AdInline documentation](README.md#set-adinline-parameters)).
Furthermore, AdHesion ads have too additional parameters `showCloseButton` and `hidesOnSwipe`.

* `showCloseButton` - defines if close button should be visible on placement. By default 
close button is hidden.

* `hidesOnSwipe` - enables/disables auto hide feature. If enabled ad will automatically 
hide when user interacts with the application and appear again when the interaction is finished.
By default this feature is turned off.

### Show AdHesion

To show AdHesion ads you have to call showAd `method`.

```c#
	adHesion.showAd ();
```

## AdOverlay integration

### Create AdOverlay

Create AdOverlay object using `AdformAdvertisingSDK` class static method `createAdOverlay(masterTag)`, for example:

```c#
	IAdOverlay adOverlay = AdformAdvertisingSDK.createAdOverlay (12345L);
```

### Basic Setup

You have to call `onResume`, `onPause` and `onDestroy` lifecycle methods. It is mandatory for Android applications.

```c#
	void OnApplicationPause (bool pauseStatus) 
	{
		if (adOverlay != null) {
			if (pauseStatus) {
				adOverlay.onPause ();
			} else {
				adOverlay.onResume ();
			}
		}
	}
	
	void OnDestroy() 
	{
		if (adOverlay != null) {
			adOverlay.onDestroy ();
		}
	}
```

### Set AdOverlay parameters
		
#### Controlling animations

AdOverlay display can be animated using a set of different animations.
This can be achieved using 'setPresentationStyle' method.
At the moment SDK supports these animation types:

    AnimationStyle.SLIDE - When view is presented, it slides up from the bottom.
    AnimationStyle.FADE - When view is presented, it fades in.
    AnimationStyle.NONE - Presentation is not animated. You can use this animation style if you want to disable animations.

By default, overlay ads use the AnimationStyle.FADE animation style.
	
#### Controlling dim overlay

It is possible to enable/disable application content dimming for AdOverlay placements by using 'setDimOverlayEnabled' method.
	
### Show AdOverlay

To show `AdOverlay`, execute `showAdOverlay()` method, for example:

```c#
	adOverlay.showAdOverlay();
```

## Ad Tags

An ad tag is a piece of HTML/JavaScript, XML or URL code which can be inserted into the ad view directly to load an ad creative.
Ad tags may be set to both 'AdInline' and 'AdOverlay' placements.
To set an HTML/JavaScript ad tag you can use 'setAdTag' method. 
You must set a string with banner source code to this property. For example:

```c#
	adInline.setAdTag("<body>Your html banner</body>");    
```

In order to display video ads, you have to set VAST XML document as ad tag. For example:

```c#
	adInline.setAdTag("<?xml version=\"1.0\" encoding=\"UTF-8\"?><VAST> Your VAST XML document </VAST>");    
```

An URL to VAST XML is supported as well.

```c#
	adInline.setAdTag("http://yourdomain.com/vast.xml");    
```

Please note, that Ad overlay doesn't support video ad tags. 

## Events

Both `AdInline` and `AdOverlay` contain state change events that you can register for. Example:

```c#
	adInline.OnAdLoaded += OnAdLoadedEvent;
    void OnAdLoadedEvent(object ob, EventArgs eventArgs)
	{
		Debug.Log("OnAdLoadedEvent");
	}
```
	
## Keywords

To add keywords which will be used for targeting purposes please use example below

```c#
	adInline.setKeywords (new List<string>(){"test", "test2"});
```

## Keyvalues

To add key value pairs which will be used for targeting purposes please use example below

```c#
	Dictionary<string, string> keyvalues = new Dictionary<string, string>();
	keyvalues.Add ("age", "30");
	keyvalues.Add ("gender", "male");
	adInline.setKeyValues (keyvalues);
```

Both key and value have a 200 character limit.
