# ulauncher-chrome-tabs

> Open your Chrome tabs from Ulauncher

⚠️⚠️ This extension is Deprecated in favour of [ulauncher-brotab](https://github.com/balta2ar/brotab/issues?q=is%3Aissue+is%3Aclosed) which is based on the awesome [brotab](https://github.com/balta2ar/brotab) cli tool. This should make the process of setup much easier.

---

## Demo

![demo](demo.gif)

## How it works

We use [Chrome Native messaging API](https://developer.chrome.com/apps/nativeMessaging) that makes possible for 3rd party applications to communicate with the Chrome browser. This API is essential since there is no other way we can get the list of opened tabs and do actions on them.
This application consists of 3 parts:
    * A chrome extension that exposes the list of opened Chrome tabs and Windows and allows us to interact with them.
    * An host application, implementing the Native Messaging API that makes the bridge between the Chrome Extension and the Ulauncher Extension, by exposing a websocket server for the ulauncher extension to connect.
    * The ulauncher extenstion itself.

## Requirements

* [ulauncher](https://ulauncher.io/) >= 3
* Python >= 3
* Python packages:
    * ```pip3 install websocket-client```
    * ```pip3 install git+https://github.com/Pithikos/python-websocket-server```
* Chrome Browser
* Unusued port 10200. (this is the port that the native host application websocket server will listen on)

## Install

### 1. Install ulauncher extension

Open ulauncher preferences window -> extensions -> add extension and paste the following url:

```https://github.com/brpaz/ulauncher-chrome-tabs```

### 2. Install Native messaging host manifest

This application located in "native_messaging" folder is required for native messaging to work. [Read this](https://developer.chrome.com/extensions/nativeMessaging#native-messaging-host) for technical details.

Open net.brunopaz.ulauncher.tabs.extension.json with an editor and change the "path" value to be the absolute path to the "host" executable.
It should be something like: ```/home/<youruser>/.cache/ulauncher_cache/extensions/ulauncher-chrome-tabs/native_messaging/host```

The manifest file must then be placed in "$HOME/.config/google-chrome/NativeMessagingHosts"
You can run the following script to do that for you.

```
cd native_messaging
sh install_host.sh
```

### 3. Install the Chrome extension.

* Go to "chrome://extensions/" in your Chrome Browser.
* Make sure "Developer mode" is on and click "Load unpacked". Then select the folder "chrome-ext" located in the root folder of the extension and load it.

### 4. Restart ulauncher

* Finally restart ulauncher.
* If everything went well you should be able to type "tabs" and see a list of your opened tabs. Clicking on an item, will activate the respective tab window.
* If you see "No tabs found" message it means something went wrong during the setup. Please look at your chrome extension logs.

## Notes

* Due to the asyncronous nature of the application, tabs will be refreshed every 30s. It seems a reasonable value but it might be reduced if needed.

## Development

```
make link
```

To see your changes, stop ulauncher and run it from the command line with: ```ulauncher -v```.

## License

MIT
