Introduction
=================

JGLayoutDotSyntax provides classes and categories to make it easier to add NSLayoutConstraints. Rather than entering lengthy code that specifies the views, attributes, relationship, constant, and mulplier all in a single function (not to mention the reciever view that must add the constraint), JGLayoutDotSyntax allows relationships to be specified simply and concisely.

Additionally, JGLayoutDotSyntax adds support for font size constraints by use of the JGDynamicSizeLabel subclass of UILabel. With this class, one can simply and set up the font size to adjust with the rest of the layout.

Finally, JGLayoutDotSyntax supports using arbitrary properties as constraints. For example, if you have a downloader object that has a property indicating the download progress, you can see the width of a view (as an example) to the progress value, and it will update with the property! JGLayoutDotSytax add not just convenience, but also very powerful new` features to autolayout.

Format
=================

Layout constraints can be specified simply and easily using reading dot syntax. The lenghty, hard-to-understand layout code

```swift
view.addConstraint(NSLayoutConstraint(item: subview, attribute: .CenterX, relatedBy: .Equal, toItem: self, attribute: .CenterX, multiplier: 1.0, constant: 0.0))
```

can be rewritten in a short, simple, easily understood format using JGLayoutDotSyntax:

```swift
subview.layoutCenterX = view.layoutCenterX
```


Conventional methods of creating autolayout constraints simply obfuscuate the intent, causing difficult to read and often buggy code. JGLayoutDotSyntax aims to fix that. The advantages achieved are even more stunning when you realize that each subview usually has about four NSLayoutConstraints created to define its location.

JGLayoutDotSyntax supports all features that NSLayoutConstraint does, including constants, multipliers, and priority. To add or subtract a constant to a constraint, simply use the plus (`+`) or minus (`-`) symbol:

```swift
subview.layoutLeft = view.layoutLeft + 10.0
subview.layoutRight = view.layoutRight - 10.0
```

Any number object (e.g. Double, Float, Int, NSNumber) can be added or subtracted as a constant to a NSLayoutConstraint. Similiarly, multipliers can be specified with the multiplication (`*`) symbol, and different relationships (i.e. `NSLayoutRelation`), can be specified using the `withRelation()` method. For more info, refer to the documentation in the `JGLayoutConstruction.swift` file.

In cases where a constant size needs to be specified without a constraint, wrap the desired number object with the typealias `JGLP()` before setting the size:

```swift
subview.layoutWidth = JGLP(42.0)
```

Additionally, JGLayoutDotSyntax allows priority to be specified. In favor of concision, a slightly irregular syntax is used. After a JGLayoutParameter, the subscript operator (`[]`) can be used to specifiy priority of a constraint, if needed. For example, we can lower the priority of centering our subview:

```swift
subview.layoutCenterX = view.layoutCenterX[UILayoutPriorityDefaultLow]
```

The argument between the brackets should be a UILayoutPriority, which is represented by a positive integer, less than or equal to 1000 (as specified in Apple's NSLayoutConstraint documentation).

Further, there exists convenience methods `layoutAlignment` and `layoutSize` and `layoutCenter` to quickly set the top, bottom, left, right constraints or the width and height of the sender to that of the receiver.

JGDynamicSizeLabel
=================

One of the coolest parts of JGLayoutDotSyntax is the `JGDynamicSizeLabel` subclass of UILabel. With it, font sizes can be linked to layout constraints effortlessly. Check it out!

```swift
var label = JGDynamicSizeLabel()
label.fontSize = view.layoutHeight * 0.5
```

In the above example, we set the label to have a font with half the value of view's height.

Installation
=================

To use JGLayoutDotSyntax with your project, you simply need to add the neccessary files to your project. It's as simple as that!

Example
=================

In order to better illustrate how JGLayoutDotSyntax is to be used, an example project is included that makes use of this syntax. Below is the relevant section of the example project:

```swift
let size = JGLP(40.0)
let statusBarHeight = UIApplication.sharedApplication().statusBarFrame.size.height

purpleView.layoutWidth = JGLP(size.constant * 2.0)
purpleView.layoutHeight = JGLP(size.constant * 2.0)
purpleView.layoutRight = view.layoutRight
purpleView.layoutTop = view.layoutTop + statusBarHeight

blueView.layoutLeft = view.layoutLeft
blueView.layoutCenterY = view.layoutCenterY
blueView.layoutHeight = size
blueView.layoutWidth = JGLP(190.0)

redView.layoutWidth = size
redView.layoutHeight = size
redView.layoutCenterX = view.layoutCenterX[UILayoutPriorityDefaultHigh]
redView.layoutCenterY = view.layoutCenterY
redView.layoutLeft = (blueView.layoutRight + 10.0).withRelation(.GreaterThanOrEqual)

let margin = 10.0

yellowView.layoutLeft = blueView.layoutLeft + margin
yellowView.layoutRight = blueView.layoutRight - margin
yellowView.layoutTop = blueView.layoutTop + margin
yellowView.layoutBottom = blueView.layoutBottom - margin

greenView.layoutBottom = view.layoutBottom
greenView.layoutHeight = view.layoutHeight * 0.2
greenView.layoutLeft = view.layoutLeft
greenView.layoutRight = view.layoutRight

label.layoutAlignment = greenView.layoutAlignment
label.fontSize = greenView.layoutHeight * 0.5
```

Displayed on a portrait-oriented and a landscape-oriented iPhone, the above layout would look like the images below:

![](https://github.com/JadenGeller/JGLayoutDotSyntax/blob/master/example_layout_portrait.png?raw=true)    
![](https://github.com/JadenGeller/JGLayoutDotSyntax/blob/master/example_layout_landscape.png?raw=true)
