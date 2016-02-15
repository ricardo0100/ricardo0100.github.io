---
layout: post
title:  "Introduction to Views and View Controllers"
date:   2016-02-15
categories:
description: Basic description about some important UIKit objects (UIView, UIViewController and UINavigationController) and the responsibilities of each one.
---

###Introduction

...


###Interface Builder

Everything that you can do with iOS framework can be done using only code, but of course

![Interface Builder](/assets/posts/interface-builder.png)

The easiest way to create user interfaces for iOS app is using Interface Builder.

<!-- - Drag and drop views
- Set some view's properties
- Manage its sizing and positioning with [autolayout][Autolayout Guide]
- Create [outlets][Outlet Docs] to link views to the code -->


###UIView
>A view is an instance of the `UIView` class and manages a rectangular area in your application window. Views are responsible for drawing content, handling multitouch events, and managing the layout of any subviews. <small>[View Programming Guide][View Programming Guide]</small>

Views are the main way of an app to interact with users. Everything you see on an app's screen is a view. Usually there are a lot of views interacting between themselves and with the user. `UILabel`, `UITextField` and `UIImageView` are some examples of views that UIKit provides.

####Frames

Every view on the screen is responsible to manage its content. All the view's content is inside a rectangular area called frame. Frame is the area that the view is presented. This area is represented by an instance of `CGRect` class where properties are set to define the coordinates (x, y) and the size (width, height) of the view.

####Responsibilities of the views:

- Manage its layout and its subviews layouts
- Detect user interaction
- Draw and animate content on the screen

Another important thing about Views is the View's hierarchy. Every instance of `UIView` can act as a container of other instances of `UIView`.

####Managing views

Interface Builder is awesome but eventually its necessary to have access to the views programatically.

To create a view use:

{% highlight swift %}
let view = UIView()
{% endhighlight %}

To create a view and define its size and position use:

{% highlight swift %}
view = UIView(frame: CGRect(x: 0, y: 0, width: 100, height: 40))
view.backgroundColor = UIColor.greenColor()
{% endhighlight %}
<small>*Result:*</small>
![](/assets/posts/uiview-1.png)

To add subviews to that view use:

{% highlight swift %}
let subview = UIView(frame: CGRect(x: 10, y: 10, width: 20, height: 20))
subview.backgroundColor = UIColor.redColor()
view.addSubview(subview)

let label = UILabel(frame: CGRect(x: 40, y: 10, width: 60, height: 20))
label.text = "Hey"
view.addSubview(label)
{% endhighlight %}
<small>*Result:*</small>
![](/assets/posts/uiview-2.png)

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

[UIView Class Reference][UIView Docs] is a great place to learn more details about managing views and its subclasses.


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
- Update the views
- Hanlde presentation of other view controllers

####Subclassing UIViewController

[rever]
During the presentation of a view controller iOS calls some methods that are used to set up and prepare the view controller to show itself to the user.

{% highlight swift %}
override func viewDidLoad() {
    super.viewDidLoad()
    //This method is called once the view controller is ready and all of its outlets are loaded, it's a good place to set up the views and datasources
}

override func viewWillAppear(animated: Bool) {
    super.viewWillAppear(animated)
    //This method is called every time the view controller becomes visible to the user, its a good place to update data that might have changed
}
{% endhighlight %}

>There are a lot of methods like this that can be overridden. Use the [UIViewController class reference][UIViewController Docs] to choose the right one for what you need and be careful to avoid performance issues.


###UINavigationController

A navigation controller is a container view controller that shows view controllers under a navigation bar. The navigation controller sets up the navigation bar according to the view that is currently being presented. It's really easy to create a navigation flow using storyboards.

>A navigation controller object manages the currently displayed screens using the navigation stack, which is represented by an array of view controllers. The first view controller in the array is the root view controller. The last view controller in the array is the view controller currently being displayed. <small>[UINavigationController class reference][UINavigationController Docs]</small>


The image below is an example of how navigation controller adapts the navigation bar while the user navigates through the iOS Settings App.

![](/assets/posts/navigation-controller.png)


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



[Interface Builder Page]: https://developer.apple.com/xcode/interface-builder/
[Apple Docs]: https://developer.apple.com/library/ios/navigation/
[UIKit Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/
[Outlet Docs]: https://developer.apple.com/library/ios/documentation/General/Conceptual/Devpedia-CocoaApp/Outlet.html#//apple_ref/doc/uid/TP40009071-CH4
[UIView Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/
[UIViewController Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIViewController_Class/
[UINavigationController Docs]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationController_Class/
[View Programming Guide]: https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009503
[View Controller Programming Guide]: https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1
[Autolayout Guide]: https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/index.html
[Navigation Controllers Guide]: https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/NavigationControllers.html
