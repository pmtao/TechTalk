# How to debug Mega iOS app on your own device based on Github repo

[Home - MEGA](https://mega.io/)

# 1. Apple developer program

Firstly, you need an apple developer program, not the free version, but the paid one that include iCloud, push notifications, and other capabilities available only in the paid program.

# 2. Install Xcode and CMake
- Install [Xcode](https://itunes.apple.com/app/xcode/id497799835?mt=12) in your system.
- Install [CMake](https://cmake.org/install/), and create a symbolic link in folder '/usr/local/bin' after install so that you can run it directly in the Xcode shell:

```shell
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install
```

# 3. Clone the repo

Repo link: [https://github.com/meganz/iOS](https://github.com/meganz/iOS)

# 4. Update submodule

- open `.gitmodules` file under repo root directory, and change the URL:

1. Change url for karere submodule, use this one: https://github.com/meganz/MEGAchat.git
2. Change url for SDK submodule, use this one: https://github.com/meganz/SDK.git

- run command to update:

```shell
git submodule update --init --recursive
```

# 5. Download 3rdparty packages

1. Download the prebuilt third party dependencies from this link: [https://mega.nz/file/AENVGYjC#HhUgIOBY69zVZZtOa4e6vdySpHefnUo4GcoQYElmEo4](https://mega.nz/file/AENVGYjC#HhUgIOBY69zVZZtOa4e6vdySpHefnUo4GcoQYElmEo4)
2. Uncompress that file and move the folders `webrtc` , `include` and `lib` into `iMEGA/Vendor/sdk/bindings/ios/3rdparty`.

# 6. Modify Xcode project

- Open `iMEGA.xcworkspace`  in Xcode, wait Swift Package to update.
- Select target MEGA, change Bundle Identifier to your own `com.xxx` , and choose Automatically manage signing with your own account.
- Change default App Groups `group.mega.ios`  to your own name `group.com.xxx` , iCloud Containers make the same change `iCloud.com.xxx` .
- Do the same for other targets: MEGAPicker, MEGAPickerFileProvider, MEGAShare, MEGANotifications, MEGAWidgetExtension, MEGAIntent.

# 7. Modify source code

- Modify file `MEGANotifications.entitlements` , delete this line: `com.apple.developer.usernotifications.filtering` .
- Modify `MEGAConstants.m`  change value of MEGAGroupIdentifier to your own group id: `group.com.xxx` .
- Modify `AppGroupContainer.swift`  change value of identifier to your own group id: `group.com.xxx` .
- Modify `iMEGA/Extensions/MEGAPickerFileProvider/Info.plist` change NSExtensionFileProviderDocumentGroup to your own group id: `group.com.xxx` .

# 8. Run the project with real iPhone device but not the simulator

Running in the simulator will crash when app launch.

