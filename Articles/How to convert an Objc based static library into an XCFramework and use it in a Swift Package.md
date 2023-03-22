# How to convert an Objc based static library into an XCFramework and use it in a Swift Package

Objective-c based static library is named like `library.a` , and have some public header files.

We can use `xcodebuild -create-xcframework`  command to build XCFramework, like this:

```shell
xcodebuild -create-xcframework -library libBase.a -output ./Ba.xcframework
```

And when you want to use the XCFramework in Swift Package, you write the package like this:

```swift
let package = Package(
    name: "Networklogging",
    products: [
        .library(
            name: "Networklogging", 
			targets: ["Networklogging"]),
    ],
    dependencies: [],
    targets: [
        .target(name: "Networklogging", 
				dependencies: ["Base"]),
        .binaryTarget(name: "Base",
                      path: "Base.xcframework"),
    ]
)
```

But when we try to use the library with `import Base` , we get errors because the Swift Package don't recognize the `Base` module.

To fix this issue, we need to convert the static library into a module by placing the header files in the XCFramework and creating a new `module.modulemap` file in it.

This is the  `module.modulemap` content:

```swift
module Base {
    umbrella header "Base.h"
    export *
}
```

You import all the public headers in "Base.h" file, and just put "Base.h" file in the  `module.modulemap` .

Then create a folder called include(other names are ok), put all the public header files and `module.modulemap` in it, build XCFramework again:

```shell
xcodebuild -create-xcframework -library libBase.a -headers include -output ./MEGASDK.xcframework
```

After these, the header files will be in the XCFramework automatically, including the `module.modulemap` file.

Then open Swift Package again, we can import Base successfully this time, case it get the module information from the `module.modulemap` file.

