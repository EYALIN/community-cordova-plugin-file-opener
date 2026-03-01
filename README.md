

# A File Opener Plugin for Cordova

[![NPM version](https://img.shields.io/npm/v/community-cordova-plugin-file-opener)](https://www.npmjs.com/package/community-cordova-plugin-file-opener)
[![Downloads](https://img.shields.io/npm/dm/community-cordova-plugin-file-opener)](https://www.npmjs.com/package/community-cordova-plugin-file-opener)

#### This is a fork of the original plugin cordova-plugin-file-opener2

I dedicate a considerable amount of my free time to developing and maintaining many cordova plugins for the community ([See the list with all my maintained plugins][community_plugins]).
To help ensure this plugin is kept updated,
new features are added and bugfixes are implemented quickly,
please donate a couple of dollars (or a little more if you can stretch) as this will help me to afford to dedicate time to its maintenance.
Please consider donating if you're using this plugin in an app that makes you money,
or if you're asking for new features or priority bug fixes. Thank you!

[![](https://img.shields.io/static/v1?label=Sponsor%20Me&style=for-the-badge&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/eyalin)



This plugin will open a file on your device file system with its default application.

```js
cordova.plugins.fileOpener2.open(
    filePath,
    fileMIMEType,
    {
        error : function(){ },
        success : function(){ }
    }
);
```

## Installation

```shell
$ cordova plugin add community-cordova-plugin-file-opener
```

### Optional variables

This plugin requires the Android support library v4. From release `2.1.0` the version of this can be set at installation. The minimum version is `24.1.0`. Default value is `27.+`. [Check out the latest version](https://developer.android.com/topic/libraries/support-library/revisions.html).

```shell
$ cordova plugin add community-cordova-plugin-file-opener  --variable ANDROID_SUPPORT_V4_VERSION="27.+"
```

If you are using the `cordova-android-support-gradle-release` plugin it should match the value you have set there.

## Android Permissions

This plugin only declares `READ_EXTERNAL_STORAGE` (with `maxSdkVersion="32"`) for legacy Android support.

If your app needs to access media files on Android 13+, you must declare the required permissions in your app's `config.xml`:

```xml
<platform name="android">
    <config-file target="AndroidManifest.xml" parent="/*" xmlns:android="http://schemas.android.com/apk/res/android">
        <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
        <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
        <uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />
    </config-file>
</platform>
```

Only add the permissions your app actually needs. Google Play will reject apps that declare media permissions without a core need for persistent media access.

## Requirements

The following platforms and versions are supported by the latest release:

- Android 4.4+ / iOS 9+ / Windows / Electron
- Cordova CLI 7.0 or higher

Cordova CLI 6.0 is supported by 2.0.19, but there are a number of issues, particularly with Android builds (see [232](https://github.com/pwlin/cordova-plugin-file-opener2/issues/232) [203](https://github.com/pwlin/cordova-plugin-file-opener2/issues/203) [207](https://github.com/pwlin/cordova-plugin-file-opener2/issues/207)). Using the [cordova-android-support-gradle-release](https://github.com/dpa99c/cordova-android-support-gradle-release) plugin may help.

### AndroidX Support

Currently if your project requires AndroidX support, you need to add the following two plugins to your project:

- [cordova-plugin-androidx](https://github.com/dpa99c/cordova-plugin-androidx/) and [cordova-plugin-androidx-adapter](https://github.com/dpa99c/cordova-plugin-androidx-adapter/)
```shell
$ cordova plugin add cordova-plugin-androidx
$ cordova plugin add cordova-plugin-androidx-adapter
```
Just adding these plugins should be enough and no further changes are necessary.


## fileOpener2.open(filePath, mimeType, options)

Opens a file

### Supported Platforms

- Android 4.4+
- iOS 9+
- Windows
- Electron

### Quick Examples
Open an APK install dialog:

```javascript
cordova.plugins.fileOpener2.open(
    '/Downloads/gmail.apk',
    'application/vnd.android.package-archive'
);
```

Open a PDF document with the default PDF reader and optional callback object:

```js
cordova.plugins.fileOpener2.open(
    '/Download/starwars.pdf', // You can also use a Cordova-style file uri: cdvfile://localhost/persistent/Downloads/starwars.pdf
    'application/pdf',
    {
        error : function(e) {
            console.log('Error status: ' + e.status + ' - Error message: ' + e.message);
        },
        success : function () {
            console.log('file opened successfully');
        }
    }
);
```

__Note on Electron:__ Do not forget to enable Node.js in your app by adding `"nodeIntegration": true` to `platforms/electron/platform_www/cdv-electron-settings.json` file, See [Cordova-Electron documentation](https://cordova.apache.org/docs/en/latest/guide/platforms/electron/index.html#customizing-the-application's-window-options).

### Market place installation
Install From Market: to install an APK from a market place, such as Google Play or the App Store, you can use an `<a>` tag in combination with the `market://` protocol:

```html
<a href="market://details?id=xxxx" target="_system">Install from Google Play</a>
<a href="itms-apps://itunes.apple.com/app/my-app/idxxxxxxxx?mt=8" target="_system">Install from App Store</a>
```
or in code:

```js
window.open("[market:// or itms-apps:// link]","_system");
```

## fileOpener2.showOpenWithDialog(filePath, mimeType, options)

Opens with system modal to open file with an already installed app.

### Supported Platforms

- Android 4.4+
- iOS 9+

### Quick Example

```js
cordova.plugins.fileOpener2.showOpenWithDialog(
    '/Downloads/starwars.pdf', // You can also use a Cordova-style file uri: cdvfile://localhost/persistent/Downloads/starwars.pdf
    'application/pdf',
    {
        error : function(e) {
            console.log('Error status: ' + e.status + ' - Error message: ' + e.message);
        },
        success : function () {
            console.log('file opened successfully');
        },
        position : [0, 0]
    }
);
```
`position` array of coordinates from top-left device screen, use for iOS dialog positioning.

## fileOpener2.uninstall(packageId, callbackContext)

Uninstall a package with its ID. 

__Note__: You need to add `<uses-permission android:name="android.permission.REQUEST_DELETE_PACKAGES" />` to your `AndroidManifest.xml`

### Supported Platforms

- Android 4.4+

### Quick Example
```js
cordova.plugins.fileOpener2.uninstall('com.zynga.FarmVille2CountryEscape', {
    error : function(e) {
        console.log('Error status: ' + e.status + ' - Error message: ' + e.message);
    },
    success : function() {
        console.log('Uninstall intent activity started.');
    }
});
```

## fileOpener2.appIsInstalled(packageId, callbackContext)

Check if an app is already installed.

### Supported Platforms

- Android 4.4+

### Quick Example
```javascript
cordova.plugins.fileOpener2.appIsInstalled('com.adobe.reader', {
    success : function(res) {
        if (res.status === 0) {
            console.log('Adobe Reader is not installed.');
        } else {
            console.log('Adobe Reader is installed.')
        }
    }
});
```
---

## Android APK installation limitation

[community_plugins]: https://github.com/EYALIN?tab=repositories&q=community&type=&language=&sort=


```

- Before Android 7, you can only install APKs from the "external" partition. For example, you can install from `cordova.file.externalDataDirectory`, but **not** from `cordova.file.dataDirectory`. Android 7+ does not have this limitation.

---

## SD card limitation on Android

It is not always possible to open a file from the SD Card using this plugin on Android. This is because the underlying  Android library used [does not support serving files from secondary external storage devices](https://stackoverflow.com/questions/40318116/fileprovider-and-secondary-external-storage). Whether or not your the SD card is treated as a secondary external device depends on your particular phone's set up.

---

## Notes

- For properly opening _any_ file, you must already have a suitable reader for that particular file type installed on your device. Otherwise this will not work.

- [It is reported](https://github.com/pwlin/cordova-plugin-file-opener2/issues/2#issuecomment-41295793) that in iOS, you might need to remove `<preference name="iosPersistentFileLocation" value="Library" />` from your `config.xml`

- If you are wondering what MIME-type should you pass as the second argument to `open` function, [here is a list of all known MIME-types](http://svn.apache.org/viewvc/httpd/httpd/trunk/docs/conf/mime.types?view=co)


---


