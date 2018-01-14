# react-native-auto-updater

[![License](https://img.shields.io/cocoapods/l/ReactNativeAutoUpdater.svg?style=flat)](http://cocoapods.org/pods/ReactNativeAutoUpdater)
[![Platform](https://img.shields.io/cocoapods/p/ReactNativeAutoUpdater.svg?style=flat)](http://cocoapods.org/pods/ReactNativeAutoUpdater)

<img src="images/rnau.png" alt="React-Native Auto-Updater" width="400" />

## About

At [AeroFS](http://www.aerofs.com), we're close to shipping our first React Native app. Once the app is out, we would want to send updates over the air to bypass the sluggish AppStore review process, and speed up release cycles. We've built `react-native-auto-updater` to do just that. It was built as a part of our [2015 Thanksgiving Hackathon](https://www.aerofs.com/blog/how-we-run-hackathons/).

> **Does Apple permit this?**
>
> Yes! [Section 3.3.2 of the iOS Developer Program](https://developer.apple.com/programs/ios/information/iOS_Program_Information_4_3_15.pdf) allows it "provided that such scripts and code do not change the primary purpose of the Application by providing features or functionality that are inconsistent with the intended and advertised purpose of the Application."


> **Does Google permit this?**
>
> Of course!

React Native `jsbundle` can be easily over a couple of megabytes. On cellular connections, downloading them more often than what is needed is not a good idea. To tackle that problem, we need to decide if the bundle needs to be downloaded at all.

We solve this by shipping the app with an initial version of the `jsbundle`, this reduces the latency during the initial startup. Then we start querying for available update, and download the updated `jsbundle`. All subsequent runs of the app uses this updated bundle.

In order to decide whether to download the `jsbundle` or not, we need to know some *meta*-information about the bundle. For `react-native-auto-updater`, we store this *meta*-information as a form of a JSON file somewhere on the internet. The format of the JSON is as follows

``` json
{
	"version": "1.1.0",
	"minContainerVersion": "1.0",
	"url": {
      "url": "/s/3klfuwm74sfnj0w/main.jsbundle?raw=1",
      "isRelative": true
    }
}
```

Here's what the fields in the JSON mean:

* `version` — this is the version of the bundle file (in *major.minor.patch* format)
* `minContainerVersion` — this is the minimum version of the container (native) app that is allowed to download this bundle (this is needed because adding a new React Native component to your app might result into changed native app, hence, going through the AppStore review process)
* `url.url` — this is where `ReactNativeAutoUpdater` will download the JS bundle from
* `url.isRelative` — this tells if the provided URL is a relative URL (when set to `true`, you need to set the hostname by using the method `(void)setHostnameForRelativeDownloadURLs:(NSString*)hostname;`)

`react-native-auto-updater` needs know the location of this JSON file upon initialization.

## Screenshots

Here's a GIF'ed screencast of `react-native-auto-updater` in action.

![rn-auto-updater](https://cloud.githubusercontent.com/assets/216346/12154339/c0e6e432-b473-11e5-8aa9-29ef89029c08.gif)
![rn-auto-updater-android](https://cloud.githubusercontent.com/assets/216346/12801849/b5289690-ca94-11e5-9038-1224fac98efa.gif)


#### Important

Don't forget to provide `react-native-auto-updater` with the metadata file for the JS code that is shipped with the app. Metadata in this file is used to compare the shipped JS code with updates. Thanks to [@arbesfeld](https://github.com/arbesfeld) for pointing out this bug.


## License

`react-native-auto-updater` is available under the MIT license. See the LICENSE file for more info.
