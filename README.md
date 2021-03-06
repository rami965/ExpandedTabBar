# ExpandedTabBar v3.0.1
![Platform](https://img.shields.io/badge/platform-iOS-black.svg) ![Badge w/ Version](https://img.shields.io/cocoapods/v/ExpandedTabBar.svg)  ![Platform](https://img.shields.io/badge/Swift_Package_Manager-compatible-green.svg) 


**ExpandedTabBar** is a very creative designed solution for "more" items in UITabBarController. It's greate experience to have more comfortable and intuitive UI.

![](Resources/demo.gif)
#

- *[Requirements](#requirements)*
- *[Installation Guide](#installation)*
    - *Cocoapods*
    - *Swift Package Manager*
- *[Flow Setup](#flow-setup)*
    - *With Storyboard* 
    - *Programmatically* 
- *[Delegation](#delegation)*
- *[Customization](#customization)*
    - *More Tab* 
    - *Light/Dark Mode Support* 
    - *Options Customization* 
- *[Animation Types](#animation-types)*
- *[Indicator Types](#Indicator-types)*
- *[Examples](#examples)*
- *[Support](#support)*
- *[Let us know!](#let-us-know)*
- *[License](#license)*

## Requirements

- **iOS 11.0 +**
- **Swift 5.﹡**
- **XCode 10 +**

## Installation

**ExpandedTabBar** doesn't contain any external dependencies.

*These are currently the supported intllation options:*
  
### [CocoaPods](https://cocoapods.org/)
To integrate **ExpandedTabBar** into your Xcode project using CocoaPods, specify it in your `Podfile`:
```ruby
use_frameworks!

target '<Your Target Name>' do
    pod 'ExpandedTabBar'
end
```
### [Swift Package Manager](https://swift.org/package-manager/)

The  **Swift Package Manager** is a tool for automating the distribution of Swift code and is integrated into the  `swift`  compiler. It is in early development, but appstore-card-transition does support its use on supported platforms.

Once you have your Swift package set up, adding **ExpandedTabBar** as a dependency is as easy as adding it to the  `dependencies`  value of your  `Package.swift`.
```swift
dependencies: [
    .package(url: "https://github.com/yervandsar/ExpandedTabBar", from: "3.0.1")
]
```
 Or you can checkout [Adding Package Dependencies to Your App](https://developer.apple.com/documentation/xcode/adding_package_dependencies_to_your_app) *by **Apple***

## Flow Setup
- #### *With Storyboard*
1.  Create  `UITabBarController`
2.  Extend from  `ExpandedTabBarController`
3.  Set to  `UITabBarController`  in storyboard

- #### *Programmatically*
```swift
let viewControllers: [UIViewController] = [...] // Array of view controllers for UITabBarController
let expandedTabBar = ExpandedTabBarController()
expandedTabBar.setup(viewControllers: viewControllers)
```
**NOTE:** any `UIViewController` in `viewControllers` array must have `tabBarItem`.

## Delegation

For tab selection action implement  `ExpandedTabBarControllerDelegate`:
```swift
func expandedTabBarController(
    _ tabBarController: UITabBarController,
    didSelect viewController: UIViewController,
    withItem tabBarItem: UITabBarItem?
)
```

## Customization

- #### *More Tab*
You can customize more tab in storyboard, or set programmatically.
```swift
moreTitle        : String   // Default "More"
moreIcon         : UIImage? // Default Image from SystemItem.More
moreSelectedIcon : UIImage? // Default nil
```
- #### *Dark Mode Support*
**ExpandedTabBar** is fully ***Light/Dark*** mode supported and for setting any `UIColor` or `CGColor` you can use.
```swift
let color: UIColor/CGColor = .pattern(light: UIColor, dark: UIColor)
```
**NOTE:** If device OS version does not support dark mode, it will take light as default.
- #### *Options Customization*
**ExpandedTabBar** options conforms to **Options** protocol.

```swift
public  protocol  Options {
    var  background   : BackgroundOptions
    var  container    : ContainerOptions
    var  animationType: AnimationType
    var  indicatorType: IndicatorTypes
}
```
#### *Background Options*
```swift
public protocol  BackgroundOptions {
    var color     : UIColor // Default .clear
    var alpha     : CGFloat // Default 0.3
    var closeOnTap: Bool    // Default true
}
```
#### *Container Options*
```swift
public protocol  ContainerOptions {
    var color         : UIColor             // Default .pattern(light: .white, dark: .black)
    var alpha         : CGFloat             // Default 1.0
    var cornerRadius  : CGFloat             // Default 10
    var roundedCorners: UIRectCorner        // Default .allCorners
    var bottomMargin  : CGFloat             // Default 15
    var tabSpace      : CGFloat             // Default 8
    var tab           : ContainerTabOptions
    var shadow        : ShadowOptions?      // Default nil
}
```
**NOTE:** For shadow you can see and use `ShadowDefaultOptions` class

#### *Container's Tab Options*
```swift
public protocol  ContainerTabOptions {  
    var itemHeight     : CGFloat.           // Default 35
    var titleFont      : UIFont             // Default .systemFont(ofSize: 16)
    var titleColor     : UIColor            // Default .pattern(light: .black, dark: .white)
    var iconColor      : UIColor            // Default .pattern(light: .black, dark: .white)
    var iconContentMode: UIView.ContentMode // Default .scaleAspectFit
    var iconTitleSpace : CGFloat            // Default 8
}
```
## *Animation Types*
 
 |  NoneAnimations | Translate  | Zoom  |  Rotate |
 :--------------------:|:-----------:|:--------:|:--------:|
 |`.none` |`.top` **Default**|`.zoomIn`|`.rotatePositive`|
 |![](Resources/none.gif)|![](Resources/top.gif)|![](Resources/zoomIn.gif)|![](Resources/rotatePositive.gif)|
 |`.crossDissolve` |`.bottom`|`.zoomOut`|`.rotateNegative`|
 |![](Resources/crossDissolve.gif)|![](Resources/bottom.gif)|![](Resources/zoomOut.gif)|![](Resources/rotateNegative.gif)|
 | |`.left`|`.zoomX`|`.rotate(angle: .pi/6)`|
 | |![](Resources/left.gif)|![](Resources/zoomX.gif)|![](Resources/rotate.gif)|
 |  |`.right`|`.zoomY`| |
 | |![](Resources/right.gif)|![](Resources/zoomY.gif)| |
 
 **NOTE:** You can create your own animation `.custom(AnimationProtocol)`. You can use TransformAnimation for creating custom animations

## *Indicator Types*

| `.none` | `.connectedLine`  |  `.triangle` **Default** |
:---------:|:-----------------------:|:----------------------------:|
|![](Resources/none.png)|![](Resources/connectedLine.png)|![](Resources/triangle.png)|

## Examples
```swift
final class CustomViewController: ExpandedTabBarController {

    override func viewDidLoad() {
        super.viewDidLoad()
        expandedDelegate = self
        expandedTabBarOptions = customOptions
        
    }
    
    private var customOptions: Options {
        var options = ExpandedTabBarOptions()
        
        options.indicatorType = .connectedLine
        options.animationType = .custom(customAnimation)
        
        options.container.roundedCorners = [.topLeft, .topRight, .bottomLeft]
        options.container.cornerRadius = 20
        
        options.container.shadow = ShadowDefaultOptions()
        options.container.tabSpace = 15
        options.container.tab.iconTitleSpace = 15
        
        return options
    }
    
    private var customAnimation: AnimationProtocol {
        let transform = CGAffineTransform(scaleX: 0.1, y: 0.1).rotated(by: .pi)
        return TransformAnimation(transform: transform)
    }
    
}

extension  CustomViewController: ExpandedTabBarControllerDelegate {

    func expandedTabBarController(_ tabBarController: UITabBarController,
                                    didSelect viewController: UIViewController,
                                    withItem tabBarItem: UITabBarItem?) {
        // Do some logic here
    }
}
```

## Support

Feel free to [open issuses](https://github.com/yervandsar/ExpandedTabBar/issues/new) with any suggestions, bug reports, feature requests, questions.

## Let us know!

We’d be really happy if you sent us links to your projects where you use our component. Just send an email to yervandsar@gmail.com And do let us know if you have any questions or suggestion regarding the animation.


### License

The MIT License (MIT)

Copyright (c) 2018 Yervand Saribekyan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
