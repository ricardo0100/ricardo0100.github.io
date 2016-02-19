---
layout: post
title:  "Introduction to Views and View Controllers"
date:   2016-02-19
categories:
description: Basic description about some important UIKit objects (UIView, UIViewController and UINavigationController) and the responsibilities of each one.
---

###Introduction

When I first opened the iOS project in the company I work to add some features I got a little worried. I had no idea about how iOS development. So I started to study. And this is what this blog is about. Things that I've learned while studying iOS frameworks and I would liked to have read before getting started. I hope it helps! 🙂

This first post is about some basic concepts of **UIKit** components like **views** and **view controllers** which are the basis for every iOS app. All the references are from the [official documentation][Apple Docs].

####About iOS Technologies

[iOS][iOS Page] is the operating system that runs on iPad, iPod touch and iPhone devices. It manages the hardware and provides technologies that allow us developers to build apps.

At a glance iOS is divided in four layers:

![iOS Layers](/public/posts/ios-layers.png)

- **Cocoa Touch**: contains key frameworks for building iOS. These frameworks define the appearance of your app. They also provide the basic  infrastructure for multitasking, touch-based input, push notifications and other features.
- **Media**: contains the graphics, audio, and video technologies you use to implement multimedia experiences in your apps
- **Core Services**: contains fundamental system services for apps like iCloud, Networking, Database, and Files.
- **Core OS**: contains the low-level features that most other technologies are built upon.

**UIKit** is part of the [Cocoa Touch][Cocoa Touch] layer. It is the iOS Graphic User Interface framework. This post introduces three of the most basic UIKit concepts and their interaction with Interface Builder.

- UIView
- UIViewController
- UINavigationController

["About the iOS Technologies"][About the iOS Technologies] is a great document where you can find more information about the technologies iOS provides.


###Interface Builder

![Interface Builder](/public/posts/interface-builder.png)

####Interface Builder vs Code

Interface Builder (IB) allows developers to visually create graphic interfaces for iOS. It makes the development process of an app much faster and provides a great way to design screens and custom views. Some people prefer to make everything using only code, but I believe that you can take the most advantages of Interface Builder and still keep the possibility to make code customizations if you learn how things work.

####With Interface Builder it is possible to:

- Drag and drop views to the screen
- Set view's properties
- Manage its sizing and positioning with [Auto Layout][Auto Layout Guide]
- Create [outlets][Outlet Docs] to link views to the code
- Create screen flows using [segues][Segues Guide]
- Adapt the layout to different screens and orientations with [size classes][Size Classes Guide]

If you still need to do adjust anything that IB can't do you can finish the job using some code.

There is a WWDC video about [Implementing UI Designs in Interface Builder][Interface Builder WWDC] that shows a lot of the Interface Builder superpowers💫.

###UIView

>A view is an instance of the `UIView` class and manages a rectangular area in your application window. Views are responsible for drawing content, handling multitouch events, and managing the layout of any subviews. <small>[View Programming Guide][View Programming Guide]</small>

Views are the main way of an app to interact with users. Everything you see on an app's screen is a view. Usually there are a lot of views interacting between themselves and with the user. `UILabel`, `UITextField` and `UIImageView` are some examples of views that UIKit provides. The Interface Builder have a pane in the bottom right corner of the screen called Object Library where you can find a set of views that you can drag and drop to the screen.

![Object Library](/public/posts/object-library.png)

####Responsibilities of the Views:

- Manage its layout and its subviews layouts
- Detect user interaction
- Draw and animate content on the screen

####Frames

Every view on the screen is responsible to manage its own visual content. All the view's content is shown inside a rectangular area called frame. Let's use the `UIImageView` in the picture below as an example:

![Frames](/public/posts/frames.png)

Frames have a position related to its superview (x and y) and a size (width and height) to define the area it uses. This area is represented by an instance of `CGRect` class called `frame`. Every view on screen have this property, that's how iOS knows where to put your views when running your app. Don't worry about setting frames up. [Auto Layout][Auto Layout Guide] calculates everything for you considering screen sizes and orientation differences.

To create a view and set its frame use:

{% highlight swift %}
view = UIView(frame: CGRect(x: 0, y: 0, width: 100, height: 40))
view.backgroundColor = UIColor.greenColor()
{% endhighlight %}
<small>*Result:*</small>
![](/public/posts/uiview-1.png)

To add subviews to this view use:

{% highlight swift %}
let subview = UIView(frame: CGRect(x: 10, y: 10, width: 20, height: 20))
subview.backgroundColor = UIColor.redColor()
view.addSubview(subview)

let label = UILabel(frame: CGRect(x: 40, y: 10, width: 60, height: 20))
label.text = "Hey"
view.addSubview(label)
{% endhighlight %}
<small>*Result:*</small>
![](/public/posts/uiview-2.png)

####View's hierarchy

Every instance of `UIView` can act as a container of other instances of `UIView`. It looks something like a tree data structure.

![Views Tree](/public/posts/views-tree.jpg)

To access the subviews of a view use:

{% highlight swift %}
let subviews = view.subviews
//returns an array of UIViews
{% endhighlight %}

To access the superview of a view use:
{% highlight swift %}
let superview = view.superview
//returns an instance of UIView or nil
{% endhighlight %}

####Customizing Views

To customize a view you just need to subclass any `UIView`. A good example of view customization is when we want to use prototype cells for table views. Imagine a table view cell in the Interface Builder like the one highlighted in green:

![Wheather Cell](/public/posts/wheather-cell.png)

To link the cell with the managing class for it use the Identity Inspector in the right pane of Interface Builder and choose a subclass of `UITableViewCell`.

![Identity Inspector](/public/posts/cell-identity-inspector.png)

`WeatherForDateTableViewCell` is the class created to manage the views that you dragged onto the cell prototype.

{% highlight swift %}

{% endhighlight %}

If you want to learn more about what `UIView` class can do use the [UIView Class Reference][UIView Docs]. I also recommend the [View Programming Guide for iOS][View Programming Guide] to learn more details about managing views.


###UIViewController

A view controller is a class that manages a portion of views of your app, usually a whole screen. For example, in a weather app a list of cities could be a view controller and when a city is selected the weather details for that city appears in another view controller.

>The `UIViewController` class defines the methods and properties for managing your views, handling events, transitioning from one view controller to another, and coordinating with other parts of your app. <small>[View Controller Programming Guide][View Controller Programming Guide]</small>

####Responsibilities of the view controllers:

- Set up views
- Update views content
- Manage interactions between views
- Handle user events
- Manage transitioning between view controllers

####Types of view controllers:

- *Content view controllers* manage the content views inside a specific view controller
- *Container view controllers* provide a way to present other view controllers. Some examples: `UINavigationController`, `UITabBarController` and `UISplitViewController`

####Managing view controllers

Every view controller is handled by a subclass of `UIViewController`. The role of this class is:

- Manage data from the model layer
- Provide callbacks to user interactions
- Set up views
- Update views
- Handle presentation of other view controllers

####Subclassing UIViewController

Every view controller in an iOS app is managed by an instance of `UIViewController` or a subclass. You can create everything programmatically using something like:

{% highlight swift %}
let viewController = UIViewController()
let label = UILabel(frame: CGRect(x: 40, y: 10, width: 60, height: 20))
label.text = "Hey"
viewController.view.addSubview(label)
{% endhighlight %}

[Review] When you create a class inherited from `UIViewController` it appears as an option in the class property in the Identity Inspector for the view controllers in the Interface Builder. So when your app uses an instance of that view controller created in the Interface Builder it will be managed by this class.

When a view controller is presented to the user iOS calls some methods to allow us to customize the view controller's behavior. Some are called more than once, so it's important to be careful with things you put in there. Use [UIViewController class reference][UIViewController Docs] to choose the right one for what you need and be careful to avoid performance issues.

{% highlight swift %}
class UserLoginViewController: UIViewController {

  override func viewDidLoad() {
      super.viewDidLoad()
      //This method is called when views are ready and all of its outlets are loaded, it's a good place to set up additional views properties like callbacks to view events.
  }

  override func viewWillAppear(animated: Bool) {
      super.viewWillAppear(animated)
      //This one is called every time the view controller is about to become visible to the user. You could update data in the views that might have changed while the view was not visible for example.
  }

}
{% endhighlight %}


###UINavigationController

A navigation controller is a container view controller that shows its child view controllers under a navigation bar. The navigation controller sets up the navigation bar according to the view controller that is currently being presented. Every view controller has a property called `navigationItem` of the type `UINavigationItem`. This is the object that navigation controller uses to know what to show in the navigation bar (e.g. Title). See the [UINavigationItem class reference][UINavigationItem Docs] to

>A navigation controller object manages the currently displayed screens using the navigation stack, which is represented by an array of view controllers. The first view controller in the array is the root view controller. The last view controller in the array is the view controller currently being displayed. <small>[UINavigationController class reference][UINavigationController Docs]</small>

![IB Navigation Controller](/public/posts/ib-navigation-controller.png)


[Explicar imagem]

####Managing the navigation controller

It's possible to set up almost all the navigation flow using Interface Builder, but eventually coding is inevitable.

To access the child view controllers use:

{% highlight swift %}
let viewControllers = navigationController.viewControllers
//Returns an array of UIViewController
{% endhighlight %}

To push a view controller to the top of the navigation stack use:

{% highlight swift %}
navigationController.pushViewController(viewController, animated: true)
{% endhighlight %}

To remove the currently visible view controller from the top of the navigation stack use:

{% highlight swift %}
navigationController.popViewControllerAnimated(true)
{% endhighlight %}

When the navigation flow is defined using storyboards and segues you might want to prepera the view controller to be shown before it becomes visible. To do that override the `prepareForSegue` method like:

{% highlight swift %}
override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
    let destinationViewController = segue.destinationViewController
    //Set up the destinationViewController
}
{% endhighlight %}

More about navigation controllers can be found in the [Navigation Controllers Guide][Navigation Controllers Guide].


###Conclusion

...




[iOS Page]: http://www.apple.com/ios/
[Interface Builder WWDC]: https://developer.apple.com/videos/play/wwdc2015-407/
[Interface Builder Guide]: https://developer.apple.com/library/ios/recipes/xcode_help-interface_builder/_index.html
[Apple Docs]: https://developer.apple.com/library/ios/navigation/
[UIKit Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/
[Outlet Docs]: https://developer.apple.com/library/ios/documentation/General/Conceptual/Devpedia-CocoaApp/Outlet.html#//apple_ref/doc/uid/TP40009071-CH4
[UIView Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/
[UIViewController Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIViewController_Class/
[UINavigationController Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationController_Class/
[View Programming Guide]: https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009503
[View Controller Programming Guide]: https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1
[Auto Layout Guide]: https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/index.html
[Navigation Controllers Guide]: https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/NavigationControllers.html
[Segues Guide]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_storyboard/Chapters/StoryboardSegue.html
[Size Classes Guide]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_adaptive_sizes/chapters/AboutAdaptiveSizeDesign.html
[Cocoa Touch]: https://developer.apple.com/library/ios/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/iPhoneOSTechnologies/iPhoneOSTechnologies.html
[UINavigationItem Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationItem_Class/
[MVC]: https://developer.apple.com/library/mac/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html
[About the iOS Technologies]: https://developer.apple.com/library/ios/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007898-CH1-SW1
