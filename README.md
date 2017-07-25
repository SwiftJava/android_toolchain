# Swift toolchain for Android

![](http://johnholdsworth.com/swifthello.jpg)

This is the issue tracking repo for the Ubuntu 16.04 binaries for including Swift into an Android application which can be downloaded [here](http://johnholdsworth.com/android_toolchain.tgz). This link contains a README which will direct you to the current version hosted on Goole Drive. It's intended to be used in hybrid Java/Swift applications, an example of which is [swift-android-samples](https://github.com/SwiftJava/swift-android-samples).

After you have downloaded the toolchain, setup your environment using the script `setup_ubuntu_client.sh` in the release which downloads the Android NDK and sets up links from `/usr/android/ndk` required for system headers.

It should just then be a case of cloning the [swift-android-gradle](https://github.com/SwiftJava/swift-android-gradle) and typing `./gradlew install` then cloning swift-android-samples and typing `cd swifthello; ./gradlew installDebug`. This uses the Swift package manager to build the a shared library and copies in those for swift itself. Make sure the `.../swift-install/usr/bin` directory is in your path.

There are a couple of Android specific APIs in this release which need to be used to setup thread cleanup, the TMPDIR environment variable and a Certificate Authority "pem" file in order to be able to use SSL/https:. These are setup in the example project [here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/java/net/zhuoweizhang/swifthello/SwiftHello.java#L35) and [here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/swift/Sources/main.swift#L20).

A hybrid app is a conventional Android Java app that loads a `swifthello.so` dynamic library on initialisation. The developer specifies a pair of Java interfaces to communicate to and from Swift and a code generator generates Swift to realise the JNI calls to implements these protocols. The interface for messaging from Java to Swift must have a name that ends in "Listener" and the interface in the opposite direction is the "Responder".

There is one final JNI hook that needs to be code by hand to bind the Listener and Responder to each end of the pipe. This is [the code here](https://github.com/SwiftJava/swift-android-samples/blob/master/swifthello/src/main/swift/Sources/main.swift#L10).

## Status

This toolchain could be considered to be at a fairly well advanced Beta stage with a view to updating it to a 1.0 release when Swift 4.0 is finalised. Please take the time to report any issues you encounter, preferably with an simple example project if possible on this repo.

## Building the toolchain

I'd not recommend trying this as it is by no means an automated process at present. libdispatch needs have the TRASHIT macro defined in `src/internal.h` and it's linking modified to link against `-lswiftCore` and `-latomic` with the appropriate paths. At the time of writing, there are also a few currently open PRs you'll need to merge in [1113](https://github.com/apple/swift-corelibs-foundation/pull/1113), [10836](https://github.com/apple/swift/pull/10836) and [55](https://github.com/apple/swift-llvm/pull/55).

## Licenses

This binary package contains software built from the following repositories:

[https://github.com/curl/curl](https://github.com/curl/curl)

git://git.gnome.org/libxml2 or
[https://github.com/android/platform_external_libxml2](https://github.com/android/platform_external_libxml2)

[https://github.com/SwiftAndroid/libiconv-libicu-android](https://github.com/SwiftAndroid/libiconv-libicu-android)

[https://github.com/apple/swift](https://github.com/apple/swift)

[https://github.com/apple/swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)

[https://github.com/apple/swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation)

Built using a fork of the very handy scripts in:
[https://github.com/gonzalolarralde/swifty-robot-environment](https://github.com/gonzalolarralde/swifty-robot-environment)

Your use of this software is subject to the license agreements of these projects and,

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR 
PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE 
FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, 
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
