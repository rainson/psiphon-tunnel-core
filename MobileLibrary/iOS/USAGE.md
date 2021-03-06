# Using the Psiphon iOS Library

## Overview

Psiphon Library for iOS enables you to easily embed Psiphon in your iOS app.
You can then tunnel requests through Psiphon, ensuring that your app can't be
blocked by censors.

The Psiphon Library is available as a `.framework` that can be easily included
in your project using these instructions.

## Using the Psiphon network

In order to use a Psiphon library over the Psiphon network, you need to contact Psiphon to obtain connection parameters to use with your application. Please email us at [info@psiphon.ca](mailto:info@psiphon.ca).

## Using the Library in your App

**First step:** Review the sample app, located under `SampleApps`.
This code is a canonical guide for integrating the Library.

**Second step:** Review the comments in [`PsiphonTunnel.h`](PsiphonTunnel/PsiphonTunnel/PsiphonTunnel.h). They describe the interface and delegate requirements.

### Setting up your project

1. Get the latest iOS release from the project's [Releases](https://github.com/Psiphon-Labs/psiphon-tunnel-core/releases) page.

2. Add `PsiphonTunnel.framework` to project (drag into project tree).

3. In the "General" settings for the target, set "Deployment Target" to 9.3.

4. In the "Build Settings" for the target, under "Build Options", set "Enable Bitcode" to "No".

5. In the "Build Settings" for the target, click the `+` at the top, then "Add User-Defined Setting". Name the new setting `STRIP_BITCODE_FROM_COPIED_FILES` and set it to `NO`.

6. In the "Build Phases" for the target, add a "Copy Files" phase. Set "Destination" to "Frameworks". Add `PsiphonTunnel.framework` to the list. Ensure "Code Sign on Copy" is checked.

7. In the "Build Phases" for the target, add a "Run Script" phase. Set the script contents to:

   ```no-highlight
   bash "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/PsiphonTunnel.framework/strip-frameworks.sh"
   ```

   This step is required to work around an [App Store submission validation bug](http://www.openradar.me/23681704) that rejects apps containing a framework with simulator slices.

## Compiling and testing

The following architecture targets are compiled into the Library's framework binary: `armv7`, `arm64`, and `x86_64`. This means that the Library can run on phones or in a simulator (on a 64-bit host system).

When run in a simulator, there may be errors shown in the device log. This does not seem to affect the execution of the app (or Library).

## Proxying a web view

`WKWebView` _cannot_ be proxied. `UIWebView` _can_ be. Some [googling](https://www.google.ca/search?q=uiwebview+nsurlprotocol+proxy) should provide many example of how to do this. Here is some extensive information for [Objective-C](https://www.raywenderlich.com/59982/nsurlprotocol-tutorial) and [Swift](https://www.raywenderlich.com/76735/using-nsurlprotocol-swift).

## Other notes

If you encounter an app crash due to `SIGPIPE`, please let us know. This occurs in the debugger, but it's not clear if it happens in a production app (or is a problem). If you encounter a `SIGPIPE` breakpoint while running under the debugger, follow [these instructions](https://plus.google.com/113241179738681655641/posts/BmMiY8mpsB7) to disable it.
