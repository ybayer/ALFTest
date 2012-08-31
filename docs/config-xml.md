# Using config.xml

  Apps built using Adobe® PhoneGap™ Build can be set up either through our web interface, or by using a `config.xml`. The `config.xml` file, as specified in the [W3C widget specification](http://www.w3.org/TR/widgets/), allows developers to easily specify metadata about their applications. You can see a sample `config.xml` with our [PhoneGap Start](https://github.com/phonegap/phonegap-start/blob/master/config.xml) application.

  One thing to note: please ensure that your `config.xml` file is at the top level of your application (the same level as your `index.html` file). Otherwise it will not be loaded correctly.

  We're continually adding features to cour `config.xml` support, to give PhoneGap Build developers more power to customize their apps. If there are any specific features you'd like to see support for, [please let us know](http://getsatisfaction.com/nitobi/products/nitobi_phonegap_build).

## Essential Properties

* `<widget>`: The widget element must be the root of your XML document - it lets us know that you are following the W3C specification. When using PhoneGap Build, ensure you have the following attributes set on your widget element:
  * `id`: the unique identifier for your application. To support all supported platforms, this *must* be reverse-domain name style (e.g. `com.yourcompany.yourapp`)
  * `version`: for best results, use a major/minor/patch style version, with three numbers, such as `0.0.1`
  * `versionCode`: (optional) when building for Android, you can set the versionCode by specifying it in your *config.xml*. For more information on Android's versionCode attribute, see [the Android documentation](http://developer.android.com/guide/publishing/versioning.html). 

* `<name>`: The name of the application.

* `<description>`: A description for your application. 

#### Notes:
  
  BlackBerry only supports latin characters in the `<name>` attribute.

  BlackBerry should keep the `<description>` element at a reasonable length

#### Usage:
    
    <?xml version="1.0" encoding="UTF-8" ?>
        <widget xmlns = "http://www.w3.org/ns/widgets"
            xmlns:gap = "http://phonegap.com/ns/1.0"
            id        = "com.phonegap.example"
            versionCode="10" 
            version   = "1.0.0">
        
        <!-- versionCode is optional and Android only -->

        <name>PhoneGap Example</name>

        <description>
            An example for phonegap build docs. 
        </description>

        <author href="https://build.phonegap.com" email="support@phonegap.com">
            Hardeep Shoker 
        </author>

    </widget>

<a name="preferences"></a>
## Preferences

* `<preference>`:You can have zero or more of these elements present in your `config.xml`. If you specify none, default properties maybe applied.
  * `name`: required
  * `value`: required

### Multi-Platform

#### PhoneGap Version

* `phonegap-version`, with your chosen release of PhoneGap
  * example: `<preference name="phonegap-version" value="1.2.0" />`
  * currently supported versions are __1.1.0__, __1.2.0__, __1.3.0__, __1.4.1__,
__1.5.0__, __1.6.1__, __1.7.0__, __1.8.1__, __1.9.0__ and __2.0.0__. The default is currently __2.0.0__
    * __1.4.0__ is a deprecated release - if you wish to target 1.4.x, please ensure that you use __1.4.1__
  * If you specify a `phonegap-version` preference, all builds will be set to that version. If you do not, your app will be set to the current default version. Either way, it can be changed at a later date.
  * If you specify an unsupported version number, your app will not build

#### Device Orientation

* `orientation` with possible values `default`, `landscape`, or `portrait`
  * example: `<preference name="orientation" value="landscape" />`
  * please note that `default` means __both__ landscape and portrait are enabled. If you want to use each platform's default settings (usually portrait-only), just remove this tag from your `config.xml`

#### Target a Specific Device

* `target-device` with possible values `handset`, `tablet`, or `universal`
  * example: `<preference name="target-device" value="universal" />`
  * please note that this currently only applies to iOS builds; by default all builds are universal 

#### Fullscreen Mode

* `fullscreen` with values `true` or `false`
  * example: `<preference name="fullscreen" value="true" />`
  * hides the status bar at the top of the screen; is false by default
  * supported on iOS and Android (using PhoneGap 1.3.0 or above)

<a name="ios-prefs"></a>
### iOS Specific

#### WebView Bounce

* `webviewbounce` with values `true` or `false`
  * example: `<preference name="webviewbounce" value="false" />`
  * controls whether the screen "bounces" when scrolled beyond the top or bottom on iOS. By default, the bounce is _on_
  * support on PhoneGap 1.3.0 and above

#### Prerendered Icon

* `prerendered-icon` with values `true` or `false`
  * example: `<preference name="prerendered-icon" value="true" />`
  * if icon is prerendered, iOS will not apply it's gloss to the app's icon on the user's home screen
  * default is _false_

#### Open all links in WebView

* `stay-in-webview` with values `true` or `false`
  * example: `<preference name="stay-in-webview" value="true" />`
  * if set to true, all links (even with target set to blank) will open in the app's webview
  * only use this preference if you want pages from your server to take over your entire app
  * default is _false_

<a name="ios-statusbarstyle"></a>
#### Status Bar Style

* `ios-statusbarstyle` with values `default`, `black-opaque` or `black-translucent`
  * example: `<preference name="ios-statusbarstyle" value="black-opaque" />`
  * default is a grey status bar, `black-opaque` will appear black
  * although `black-translucent` is supported, the PhoneGap webview does not
extend beneath the status bar, so it will appear identical to `black-opaque`
once your app is running

#### Exit On Suspend

* `exit-on-suspend` with values `true` or `false`
  * example: `<preference name="exit-on-suspend" value="false" />`
  * if set to false, app will continue to run on suspend
  * default is _true_

#### Show Splash Screen Spinner

* `show-splash-screen-spinner` with values `true` or `false`
  * example: `<preference name="show-splash-screen-spinner" value="false" />`
  * if set to false, the spinner won't appear on the splash screen during app loading
  * default is _true_

#### Auto-Hide Splash Screen

* `auto-hide-splash-screen` with values `true` or `false`
  * example: `<preference name="auto-hide-splash-screen" value="false" />`
  * if set to false, the splash screen must be hidden using a JavaScript API
  * default is _true_

<a name="bb-prefs"></a>
### BlackBerry Specific

#### Disable Cursor

* `disable-cursor` with values `true` or `false`
  * example: `<preference name="disable-cursor" value="true" />`
  * prevents a mouse-icon/cursor from being displayed on the app - desugars to `<rim:navigation />`. See [the BlackBerry documentation](https://bdsc.webapps.blackberry.com/html5/documentation/ww_developing/rim_navigation_element_1582456_11.html) for more details
  * default is _false_

<a name="android-prefs"></a>
### Android Specific

#### Minimum and Maximum SDK Version

* `android-minSdkVersion` and/or `android-maxSdkVersion`, with integer values
  * minSdkVersion example: `<preference name="android-minSdkVersion" value="10" />`
  * maxSdkVersion example: `<preference name="android-maxSdkVersion" value="15" />`
  * corresponds to the `usesSdk` attributes in the `AndroidManifest.xml` file - more details are in [the Android documentation](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html)
  * minSdkVersion defaults to 7 (Android 2.1); maxSdkVersion is unset by default

#### Install Location

* `android-installLocation` with values `internalOnly`, `auto` or `preferExternal`
  * example: `<preference name="android-installLocation" value="auto" />`
  * where an app can be installed - defaults to `internalOnly` (as the Android SDK)
  * `auto` or `preferExternal` allow the app to be installed on an SD card - this can lead to unexpected behavior
  * more details available in [the Android documentation](http://developer.android.com/guide/appendix/install-location.html)

## Other Useful Elements

<a name="icons"></a>
### Icon Support

* `<icon>`: You can have zero or more of these elements present in your `config.xml`. If you do not specify a icon then the PhoneGap logo will be used as your application's icon. 
  * `src`: (required) specifies the location of the image file, relative to your `www` directory
  * `width`: (optional) but recommended to include, width in pixels
  * `height`: (optional) but recommended to include, height in pixels

#### Usage and Additional Information:

Unless otherwise specified in a config.xml, each platform will try to use the
default `icon.png` during compilation. To define platform specific icons please
use the guide provided below.

Icon files should be the file formats specified in the examples below, other file
types are not guaranteed to work across platforms.

#### Default

  The default icon must be named `icon.png` and must reside in the root of your application folder.

    <icon src="icon.png" />

#### iOS

  We support classic, retina, and iPad displays. The following will define icons
for each specific screen type.

    <icon src="icons/ios/icon.png" width="57" height="57" />
    <icon src="icons/ios/icon-72.png" gap:platform="ios" width="72" height="72" />
    <icon src="icons/ios/icon_at_2x.png" width="114" height="114" />

#### Android

  We support ldpi, mdpi, hdpi, and xhdpi displays; the following will define icons for
 each specific screen type.

    <icon src="icons/android/ldpi.png" gap:platform="android" gap:density="ldpi" />
    <icon src="icons/android/mdpi.png" gap:platform="android" gap:density="mdpi" />
    <icon src="icons/android/hdpi.png" gap:platform="android" gap:density="hdpi" />
    <icon src="icons/android/xhdpi.png" gap:platform="android" gap:density="xhdpi" />

#### BlackBerry

  BlackBerry icons __must be smaller__ then 16kb. BlackBerry also defines an
optional hover state; this state allows for a separate icon to be displayed
when a user uses the trackpad to roll over your icon image. By default the
non-hover icon will be used as the hover state.

    <icon src="icons/bb/icon.png" gap:platform="blackberry" />
    <icon src="icons/bb/icon_hover.png" gap:platform="blackberry" gap:state="hover"/>

#### Windows Phone

  We support two icons for Windows Phone, a regular icon and a tile image.

    <icon src="icons/winphone/icon.png" gap:platform="winphone" />
    <icon src="icons/winphone/tileicon.png" gap:platform="winphone" gap:role="background" />

#### WebOS 

  WebOs supports a default icon and a mini icon which can be used for notifications.

    <icon src="icons/webos/icon.png" gap:platform="webos" />
    <icon src="icons/webos/miniicon.png" gap:platform="webos" gap:role="mini" />

<a name="splashes"></a>
### Splash Screens

* `<gap:splash>`: You can have zero or more of these elements present in your
`config.xml`. This element can have `src`, `width` and `height` attributes,
just like the `<icon>` element above. Like icon files, your splash screens
should be saved as `png` files.

#### Usage and Additional Information:

Unless otherwise specified in a config.xml, each platform will try to use the
default `splash.png` during compilation. To define platform specific splash
screens please use the guide provided below.

Splash files should be the file formats specified in the examples below. Any
other file type is not guaranteed to work across platforms.

#### Default

  The default splash must be named `splash.png` and must reside in the root of your application folder.

    <gap:splash src="splash.png" />

#### iOS

  We support classic, retina, and iPad displays; the following will define
splash screens for each of those. iPads have two different splash screens,
portrait and landscape.

    <gap:splash src="splash/ios/Default.png" width="320" height="480" />
    <gap:splash src="splash/ios/Default_at_2x.png" width="640" height="960" />
    <gap:splash src="splash/ios/Default-Landscape.png" width="1024" height="768" />
    <gap:splash src="splash/ios/Default-Portrait.png" width="768" height="1024" />

#### Android

  We support ldpi, mdpi, hdpi and xhdpi displays; the following will define splash
screens for each specific screen type.

    <gap:splash src="splash/android/ldpi.png" gap:platform="android" gap:density="ldpi" />
    <gap:splash src="splash/android/mdpi.png" gap:platform="android" gap:density="mdpi" />
    <gap:splash src="splash/android/hdpi.png" gap:platform="android" gap:density="hdpi" />
    <gap:splash src="splash/android/xhdpi.png" gap:platform="android" gap:density="xhdpi" />

#### BlackBerry 

  BlackBerry supports a single splash image and can be defined as below.

    <gap:splash src="splash/bb/splash.png" gap:platform="blackberry" />

#### Windows Phone

  Windows Phone supports a single splash image and can be defined as below.
Unlike the other supported platforms, Windows Phone splash screen should be in
`jpg` format

    <gap:splash src="splash/winphone/splash.jpg" gap:platform="winphone" />

<a name="features"></a>
### PhoneGap API Features 

* `<feature>`: the feature element can be used to specify which features your application is using. If you specify features of the PhoneGap API, those will be expanded to the appropriate Android and Windows Phone permissions for you application. Currently supported through this interface are the following feature names:

  * `http://api.phonegap.com/1.0/battery`
    * maps to `android:BROADCAST_STICKY` permission

  * `http://api.phonegap.com/1.0/camera`
    * maps to `android:CAMERA`, `winphone:ID_CAP_ISV_CAMERA`, and `winphone:ID_HW_FRONTCAMERA` permissions

  * `http://api.phonegap.com/1.0/contacts`
    * maps to `android:READ_CONTACTS`, `android:WRITE_CONTACTS`, `android:GET_ACCOUNTS`,
      and `winphone:ID_CAP_CONTACTS` permissions

  * `http://api.phonegap.com/1.0/file`
    * maps to `WRITE_EXTERNAL_STORAGE` permission

  * `http://api.phonegap.com/1.0/geolocation`
    * maps to `android:ACCESS_COARSE_LOCATION`, `android:ACCESS_FINE_LOCATION`, 
      `android:ACCESS_LOCATION_EXTRA_COMMANDS`, and `winphone:ID_CAP_LOCATION` permissions

  * `http://api.phonegap.com/1.0/media`
    * maps to `android:RECORD_AUDIO`, `android:RECORD_VIDEO`, `android:MODIFY_AUDIO_SETTINGS`,
      and `winphone:ID_CAP_MICROPHONE` permissions

  * `http://api.phonegap.com/1.0/network`
    * maps to `android:ACCESS_NETWORK_STATE`, and `winphone:ID_CAP_NETWORKING` permissions

  * `http://api.phonegap.com/1.0/notification`
    * maps to `VIBRATE` permission

  * `http://api.phonegap.com/1.0/device`
    * maps to `winphone:ID_CAP_IDENTITY_DEVICE` permission

#### Usage

    <!-- If you do not want any permissions to be added to your app, add the
        following tag to your config.xml; you will still have the INTERNET
        permission on your app, which PhoneGap requires. -->
    <preference name="permissions" value="none"/>

    <!-- to enable individual permissions use the following examples -->
    <feature name="http://api.phonegap.com/1.0/battery"/>
    <feature name="http://api.phonegap.com/1.0/camera"/>
    <feature name="http://api.phonegap.com/1.0/contacts"/>
    <feature name="http://api.phonegap.com/1.0/file"/>
    <feature name="http://api.phonegap.com/1.0/geolocation"/>
    <feature name="http://api.phonegap.com/1.0/media"/>
    <feature name="http://api.phonegap.com/1.0/network"/>
    <feature name="http://api.phonegap.com/1.0/notification"/>

<a name="plugins"></a>
### Plugins

* `<gap:plugin>`: specifies a PhoneGap plugin for PhoneGap Build to include in
your generated apps.

At present, to include a plugin, you will to ensure:

* the plugin is supported by PhoneGap Build; and
* any JavaScript script tags are present in your `index.html` file.

More details, including a list of available plugins, are in our [plugins
documentation](/docs/plugins).

<a name="access"></a>
### Access Element

* `<access>`: the access element provides your app with access to resources on other domains - in particular, it allows your app to load pages from external domains that can take over your entire webview.

A blank access tag - `<access />` - denies access to any external resources. A wildcard - `<access origin="*" />` - allows access to any external resource. Otherwise, you can specify allowed domains individually:

    <access origin="https://build.phonegap.com" />

You can also allow subdomains, using the `subdomains` attribute:

    <access origin="http://phonegap.com" subdomains="true" />

To ensure that links to this domain cannot take over the entire webview, use the `browserOnly` attribute (this defaults to false):

    <access origin="http://phonegap.com" browserOnly="true" />

The behaviour of the access element is heavily dependent on the platform you're deploying to - we have a [blog post](/blog/access-tags) with more information. It is also likely to vary between different releases of PhoneGap - we'll work to maintain sane defaults and configurability for PhoneGap Build users.
