react-native home screen quick actions
======================================

Support for the new Touch 3D home screen quick actions for your React Native apps!

![](http://i.imgur.com/holmBPD.png)

_NOTE_: At this time only static quick actions are supported!

## Installing

First cd into your project's directory and grab the latest version of this code:

```bash
$ npm install react-native-quick-actions --save
```

In XCode add the library from `node_modules/react-native-quick-actions/RNQuickAction.xcodeproj` to your project then add `libRNQuickAction` to your project's __Build Phase__ > __Link Binary With Libraries__ list.

![adding to XCode](http://brentvatne.ca/images/packaging/7-add-link.gif)

Next you need to add tell XCode where it can find the RNQuickAction source.
Open up the project, add
`$(SRCROOT)/node_modules/react-native-quick-actions/RNQuickAction` to your
Header Search Paths and make sure it's `recursive`.

![](http://brentvatne.ca/images/packaging/4-header-search-paths.png)

Lastly, add the following lines to your `AppDelegate.m` file:

```obj-c
#import "RNQuickAction.h"

- (void)application:(UIApplication *)application performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem completionHandler:(void (^)(BOOL succeeded)) completionHandler {
  [RNQuickAction onQuickActionPress:shortcutItem completionHandler:completionHandler];
}
```

## Adding static quick actions

This part is pretty easy. There are a [bunch of
tutorials](https://littlebitesofcocoa.com/79) and
[docs](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/Introduction/Intro.html#//apple_ref/doc/uid/TP40016545) out there that
go over all your options, but here is the quick and dirty version:

Add these entries into to your `Info.plist` file and replace the generic stuff
(Action Title, .action, etc):

```xml
<array>
  <dict>
    <key>UIApplicationShortcutItemIconType</key>
    <string>UIApplicationShortcutIconTypeLocation</string>
    <key>UIApplicationShortcutItemTitle</key>
    <string>Action Title</string>
    <key>UIApplicationShortcutItemType</key>
    <string>$(PRODUCT_BUNDLE_IDENTIFIER).action</string>
  </dict>
</array>
```

A full list of available icons can be found here:

<https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/index.html#//apple_ref/c/tdef/UIApplicationShortcutIconType>

_NOTE_: A bunch of these icons are iOS 9.1 only so YMMV on 9.0 devices.

## Listening for quick actions in your javascript code

First, you'll need to make sure `DeviceEventEmitter` is added to the list of
requires for React.

```js
var React = require('react-native');
var {
  //....things you need plus....
  DeviceEventEmitter
} = React;

```

Use `DeviceEventEmitter` to listen for `quickActionShortcut` messages.

```js
var subscription = DeviceEventEmitter.addListener(
  'quickActionShortcut', function(data) {
    console.log(data.title);
    console.log(data.type);
    console.log(data.userInfo);
  });
```

## License

Copyright (c) 2015 Jordan Byron (http://github.com/jordanbyron/)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
