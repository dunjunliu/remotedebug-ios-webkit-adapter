# RemoteDebug iOS WebKit Adapter

[![Build Status](https://travis-ci.org/RemoteDebug/remotedebug-ios-webkit-adapter.svg?branch=master)](https://travis-ci.org/RemoteDebug/remotedebug-ios-webkit-adapter) <a href="https://github.com/RemoteDebug/remotedebug-ios-webkit-adapter/releases"><img src="https://img.shields.io/github/release/RemoteDebug/remotedebug-ios-webkit-adapter.svg" alt="Release"></a>

RemoteDebug iOS WebKit Adapter is an protocol adapter that Safari and WebViews on iOS to be debugged from tools like VS Code, Chrome DevTools, Mozilla Debugger.html and other tools compatible with the Chrome Debugging Protocol.

![](.readme/overview.png)

## Getting Started

### 1) Install dependencies

Before you use this adapter you need to make sure you have the [latest version of iTunes](http://www.apple.com/itunes/download/) installed, as we need a few libraries provided by iTunes to talk to the iOS devices.

#### Linux

Follow the instructions to install [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy#installation)  and [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)

#### Windows
All dependencies should be bundled. You should be good to go.

#### OSX/Mac
Make sure you have Homebrew installed, and run the following command to install [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy) and [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)

```
brew update
brew unlink libimobiledevice ios-webkit-debug-proxy
brew uninstall --force libimobiledevice ios-webkit-debug-proxy
brew install --HEAD libimobiledevice
brew install --HEAD ios-webkit-debug-proxy
```

### 2) Instal latest version of the adapter

```
npm install remotedebug-ios-webkit-adapter -g
```

### 3) Run the adapter from your favorite command line

```
remotedebug_ios_webkit_adapter --port=9000
```

BTW: `ios-webkit-debug-proxy` will be run automatically for you, no need to start it separately.

### 4) Open your favorite tool

Open your favorite tool such as Chrome DevTools or Visual Studio Code and configure the tool to connect to the protocol adapter.

## Configuration

```
Usage: remotedebug_ios_webkit_adapter --port [num]

Options:
  -p, --port  the adapter listerning post  [default: 9000]
  --version   prints current version

```

## Usage
### Usage with Chrome (Canary) and Chrome DevTools

You can have your iOS targets show up in Chrome's `chrome://inspect` page by leveraging the new network discoverbility feature where you simple add the IP of computer running the adapter ala `localhost:9000`.

![](.readme/chrome_inspect.png)

### Using with Mozilla debugger.html

You can have your iOS targets show up in Mozila debugger.html, by starting `remotedebug_ios_webkit_adapter --port=9222` and selecting the Chrome tab.

![](.readme/debugger_html.png)

### Using with Microsoft VS Code

Install [VS Code](https:/code.visualstudio.com), and the [VS Code Chrome Debugger](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome), then create a `launch.json` configuration where `port` is set to 9000, like below:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "iOS Web",
            "type": "chrome",
            "request": "attach",
            "port:": 9000,
            "url": "http://localhost:8080/*",
            "webRoot": "${workspaceRoot}/src"
        }
    ]
}
```

## Architecture
The protocol adapter is implemented in TypeScript as Node-based CLI tool which starts an instance of ios-webkit-debug-proxy, detects the connected iOS devices, and then starts up an instance of the correct protocol adapter depending on the iOS version.

![](.readme/architecture.png)

### Implemented methods

| Domain.method                              |
|--------------------------------------------|
| CSS.setStyleTexts                          |
| CSS.getMatchedStylesForNode                |
| CSS.addRule                                |
| CSS.getMatchedStylesForNode                |
| Page.startScreencast                       |
| Page.stopScreencast                        |
| Page.screencastFrameAck                    |
| Page.getNavigationHistory                  |
| Page.setOverlayMessage                     |
|                                            |
| DOM.enable                                 |
| DOM.setInspectMode                         |
| DOM.setInspectedNode                       |
| DOM.pushNodesByBackendIdsToFrontend        |
| DOM.getBoxModel                            |
| DOM.getNodeForLocation                     |
|                                            |
| DOMDebugger.getEventListeners              |
|                                            |
| Emulation.setTouchEmulationEnabled         |
| Emulation.setScriptExecutionDisabled       |
| Emulation.setEmulatedMedia                 |
|                                            |
| Rendering.setShowPaintRects                |
|                                            |
| Input.emulateTouchFromMouseEvent           |
|                                            |
| Network.getCookies                         |
| Network.deleteCookie                       |
| Network.setMonitoringXHREnabled            |

## How to contribute

```
npm install
npm start
```

## Diganostics logging

```
DEBUG=remotedebug npm start
```

### License
MIT
