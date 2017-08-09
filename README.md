# uproxy-apple-client-dependencies
This repository contains an Xcode project to build uProxy macOS/iOS clients dependencies.

shadowsocks-libev and tun2socks dependencies are encapsulated in `ShadowPath_[iOS|macOS].framework` and `PacketProcessor_[iOS|macOS].framework`, respectively. Additionally, ShadowPath encapsulates [Privoxy](https://www.privoxy.org) and [Antinat](http://www.malsmith.net/antinat).

## Build

This section explains how to build the dependency frameworks for macOS and iOS. After cloning the repository, run:

```
git submodule update --init
open ShadowPath.xcworkspace
```

In the XCode project, select the target to build and press `cmd+B`. Locate the built target (.framework) in the Products folder, in the file navigator. Right-click and select 'Show in Finder' to access the file.


## Update

### shadowsocks-libev

We currently depend on a [fork](https://github.com/uProxy/shadowsocks-libev-ios/) of [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev). To update it, run:

```
cd ShadowPath/shadowsocks-libev
git pull
```

**TODO: deprecate the uProxy fork and build from original shadowsocks-libev repository.**

### tun2socks

**TODO: build from original [tun2socks](https://github.com/ambrop72/badvpn/) repository.**

### libsodium

[libsodium](https://github.com/jedisct1/libsodium) is a transitive dependency of uProxy. It is required by shadowsocks-libev. To update it, run:

```
git clone git@github.com:jedisct1/libsodium.git
./libsodium/dist-build/osx.sh
./libsodium/dist-build/ios.sh
```

These commands will build static libraries and will produce two directories in `libsodium/dist-build/`, `libsodium-ios` and `libsodium-osx`. To update them in the project, run:

```
cp -R libsodium/dist-build/libsodium-osx/include ShadowPath/libsodium/
cp libsodium/dist-build/libsodium-osx/lib/libsodium.a ShadowPath/libsodium/lib-macos/
cp libsodium/dist-build/libsodium-ios/lib/libsodium.a ShadowPath/libsodium/lib-ios/
```

### libopenssl

[libopenssl](https://github.com/krzyzanowskim/OpenSSL) is a transitive dependency of uProxy. It is required by shadowsocks-libev. To update it, run:

```
git clone git@github.com:krzyzanowskim/OpenSSL.git
cp -R OpenSSL/include-macos ShadowPath/libopenssl/macos/include
cp -R OpenSSL/include-ios ShadowPath/libopenssl/ios/include
cp OpenSSL/lib-macos ShadowPath/libopenssl/lib-macos
cp OpenSSL/lib-ios ShadowPath/libopenssl/lib-ios
```

### libexpat

[libexpat](https://github.com/libexpat/libexpat/tree/master/expat) is a transitive dependency of uProxy. It is required by Antinat. To update the macOS static library and include, run:

```
git clone git@github.com:libexpat/libexpat.git
./buildconf.sh
./configure --prefix=$INSTALL_PATH
cp -R $INSTALL_PATH/include ShadowPath/Antinat/expat-lib/include
cp $INSTALL_PATH/lib/libexpat.a ShadowPath/Antinat/expat-lib/lib-macos/libexpat.a
```

## Sources
The code in this repository is adapted from [Potatso](https://github.com/uProxy/Potatso).

