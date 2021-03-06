# Pocket Change's Android SDK Plugin for Unity.

Follow the instructions below to integrate the SDK plugin.

Prerequisites:

- <a href="http://www.eclipse.org/downloads/">Android SDK</a> (version 17 or later)
- Unity (version 3.4 or later)

**Note that version 17 of the Android SDK was released in 03/2012; if you experience build errors, please ensure that you have an appropriate version.**

## Step 1: Obtain an API Key

In order to integrate the Pocket Change Android SDK plugin, you must first obtain an API key (formerly referred to as an "App ID") from your account manager. Each game will have a separate key.

## Step 2: Download the Plugin

Download the plugin from <https://github.com/pocketchange/pocketchange-android-sdk-unity-plugin/blob/master/PocketChangeAndroid.unitypackage?raw=true>.

## Step 3: Import the Plugin in Unity

To use the plugin, you must import its corresponding package in Unity: From within your Unity project, select Assets » Import Package » Custom Package and pick the location of the package you downloaded in step 2.

<img src="https://dl.dropbox.com/u/68268326/unity-plugin-doc-images/import_package.png" alt="Import Package" width="476" height="384" />

## Step 4: Create an AndroidManifest.xml

If your project does not already contain an AndroidManifest.xml file in its Plugins/Android directory, you will need to create one.

For your convenience, the Pocket Change plugin includes sample manifest files for Unity 3.4 (Unity34AndroidManifest.xml) and Unity 3.5 (Unity35AndroidManifest.xml); you can find these sample files in Plugins/PocketChangeAndroid/manifest.

Each sample manifest replaces the default manifest generated by the corresponding version of Unity, as Unity cannot auto-generate appropriate manifests for applications with plugins. You must still configure and update the sample manifests:

* Change the package attribute of the &lt;manifest&gt; element to a unique identifier for your game.
* Add the Pocket Change plugin components by following the instructions in step 5.

For additional information on Android manifest files, see <http://developer.android.com/guide/topics/manifest/manifest-intro.html>.

## Step 5: Modify your AndroidManifest.xml

Update your Plugins/Android/AndroidManifest.xml file to declare the components and permissions required by the SDK plugin. For instructions on modifying your manifest, see the <a href="https://github.com/pocketchange/pocketchange-android-sdk/blob/master/README-AndroidManifest.md" target="_blank">SDK documentation</a>.

## Step 6: Integrate the plugin with your game

To initialize the plugin, add the PocketChangeAndroidPlayerInitializer prefab to the first scene in your game; this object persists for the lifetime of your game and automatically invokes SDK lifecycle components as needed.

To configure the plugin, change the API\_KEY constant in the PocketChangeAndroidConfig script to match the API key you obtained in step 1.

Visual notifications may accompany certain rewards. In order to avoid interfering with your application, the plugin queues these notifications so that you can deliver them at convenient times. Your application must periodically display these notifications, or users will be unaware of their rewards.

To display the next pending notification, call:
```C#
PocketChangeAndroid.DisplayNotification();
```

Only invoke the `DisplayNotification` method after the player has initialized all script instances by calling their `Awake` methods (typically, you should not need any additional logic to enforce this requirement).

## Step 7: Add Event-Based Rewards

In order to reward users based on events specific to your application, you must provide your sales representative with a listing of events. Once your representative has configured the events, you can start testing the related functionality in your application.

As soon as an event occurs, invoke the following method:
```C#
PocketChangeAndroid.GrantReward(String referenceID, int amount)
```

This method informs the SDK that the event with the provided `referenceID` occurred `amount` times. Typically, `amount` should be 1; a value greater than 1 indicates multiple occurrences of the event. **Until your application goes live with events, this method will only have an effect in test mode.**

Visual notifications may accompany certain rewards. In order to avoid interfering with your application, the SDK queues these notifications so that you can deliver them at convenient times. Your application must periodically display these notifications, or users will be unaware of their rewards.

Please ensure the call to `PocketChangeAndroid.DisplayNotification` does not immediately follow the call to `GrantReward`, these functions require a small delay in which to validate the rewards with our servers before the application can display them.

Next ensure that you are displaying the queued notifications at a spot that makes sense for you app (i.e. in a level-based game at the end of each level displaying pending rewards makes sense). After invoking initialize, call the `PocketChangeAndroid.DisplayNotification` method to display any rewards accumulated since the last call to this method. The following code launches the next pending notification.
```C#
PocketChangeAndroid.DisplayNotification()
```

## <a name="testing"></a>Testing

You can use test mode to validate your integration: The plugin will grant unlimited rewards so that you can confirm your application's behavior after a reward has been granted. To enable test mode:

* In PocketChangeAndroidConfig, set the ENABLE\_TEST\_MODE constant to true.
* When building your application, check the "Development Build" box in your Android build settings (File &#187; Build Settings &#187; Android).
* Each time you switch between production and test modes, uninstall the application.

**You must disable test mode before releasing your app, otherwise users will not receive real rewards.**

The plugin only works properly on real devices. Do not use emulators for testing or you may get faulty test results.

## <a name="multi-platform-support"></a>Supporting Multiple Platforms

The Pocket Change plugin currently only supports the Android platform. If your Unity project targets multiple platforms, you must ensure that your application does not invoke any PocketChangeAndroid class methods on non-Android platforms, as such invocations will result in runtime errors. To avoid calling Android methods on non-Android platforms, use conditional compilation directives:

```C#
#if UNITY_ANDROID
PocketChangeAndroid.Method();
#endif
```

## <a name="upgrading"></a>Upgrading

To upgrade from an earlier release of the plugin:

1. Delete all files from the earlier plugin release.
2. Delete all of the previous plugin entries from your AndroidManifest.xml.
3. Complete steps 3, 5, and 6.

