# Swift toolchain for Android

![](http://johnholdsworth.com/swiftjava.png)

This is the issue tracking repo for the Android toolchain for including Swift into an Android application built on macOS X or Ubuntu Linux 16.04.

It can be downloaded [here](android_toolchain_4.1.0.tgz).

It's intended to be used in hybrid Java/Swift applications, an example of which is [swift-android-samples](https://github.com/SwiftJava/swift-android-samples) or [swift-android-kotlin](https://github.com/SwiftJava/swift-android-kotlin) .

After you have downloaded the toolchain, all that should be required is to run the script `swift-install/setup.sh` to install the gradle plugin.

Clone the example project [swift-android-samples](https://github.com/SwiftJava/swift-android-samples) and `cd swifthello; ./gradlew installDebug` with an Android Lolipop (api-21 or more recent) device to get started. This uses the Swift package manager to build the a shared library and copies in those for swift itself.

There are a couple of Android specific APIs in this release which need to be used to setup thread cleanup, the TMPDIR environment variable and a Certificate Authority "pem" file in order to be able to use SSL/https:. These are setup in the example project [here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/java/net/zhuoweizhang/swifthello/SwiftHello.java#L44) and [here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/swift/Sources/main.swift#L20).

A hybrid app is a conventional Android Java app that loads a `swifthello.so` dynamic library on initialisation. The developer specifies a pair of Java interfaces to communicate to and from Swift and a code generator [genswift.sh](https://github.com/SwiftJava/SwiftJava/blob/master/genswift.sh) generates Swift sources to realise the JNI calls to implement these protocols. The interface for messaging from Java to Swift must have a name that ends in "Listener" and the interface in the opposite direction is normally the "Responder".

There is one final JNI hook that needs to be coded by hand to bind the Listener and Responder to each end of the pipe. This is the code [here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/java/net/zhuoweizhang/swifthello/SwiftHello.java#L85) and [here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/swift/Sources/main.swift#L10).

## Status

This toolchain could be considered to be at a fairly well advanced Beta stage with a view to updating it to a 1.0 release when Swift 4.0 is finalised. Please take the time to report any issues you encounter, preferably with an simple example project if possible on this repo.

## Building the toolchain

I'd not recommend trying this at present as it is by no means an automated process. I used slightly modified versions of the [swifty-robot-environment](https://github.com/gonzalolarralde/swifty-robot-environment) scripts. libdispatch needs the most TLC. You need to make sure the TRASHIT macro is defined in `src/internal.h` and modify `src/Makefile` to link against `-lswiftCore` and `-latomic` with the appropriate paths. At the time of writing, there are also a few currently open PRs you'll need to merge in [1113](https://github.com/apple/swift-corelibs-foundation/pull/1113), [10836](https://github.com/apple/swift/pull/10836) and [55](https://github.com/apple/swift-llvm/pull/55). Preparing a macOS toolchain currently involves grafting binaries for `swift` and `swift-build` from a macOS toolchain into the Linux distribution and rebuilding `swiftc`.

## Licenses

This binary package contains software built from the following repositories:

[https://www.openssl.org](https://www.openssl.org/source/openssl-1.0.2-latest.tar.gz)

[https://github.com/curl/curl](https://github.com/curl/curl)

git://git.gnome.org/libxml2 or
[https://github.com/android/platform_external_libxml2](https://github.com/android/platform_external_libxml2)

[https://github.com/SwiftAndroid/libiconv-libicu-android](https://github.com/SwiftAndroid/libiconv-libicu-android)

[https://github.com/apple/swift](https://github.com/apple/swift)

[https://github.com/apple/swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)

[https://github.com/apple/swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation)

Built using a fork of the very handy scripts in:
[https://github.com/gonzalolarralde/swifty-robot-environment](https://github.com/gonzalolarralde/swifty-robot-environment)

This kit contains some headers and libraries from the Android NDK
[https://developer.android.com/studio/index.html](https://developer.android.com/studio/index.html)

Your use of this software is subject to the license agreements of these projects and,

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR 
PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE 
FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, 
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
