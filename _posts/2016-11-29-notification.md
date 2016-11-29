---
layout: post
title: "Notifications - Example"
date: "2016-11-29 12:30:00"
category: [iOS]
tags: [ios, notification, objective-c]
---
<div class = "message">
This is a simple example of how to post and listen to a notification radio.
<br>KF 11/29/2016
</div>

##  What is a Notification? 
>Reference: [How Best to Use Delegates and Notifications in Objective-C](http://www.informit.com/articles/article.aspx?p=2091958)

Notifications provide a means of broadcasting a message from anywhere to anywhere. The `NSNotification` class provides this functionality in Objective-C. It is not strictly a part of the language, but rather the Foundation framework. Instances of NSNotification are broadcast through an `NSNotificationCenter`.

Notifications contain a name, an object and a dictionary of metadata. The object and metadata are optional, but the name is required. An object can register with the notification center to receive certain notifications, filtered by name, object or both name and object. Additionally, a selector is passed, which is called when a notification matching the filter is broadcast.
An example of a notification is as follows:

```objc 
// Registering as observer from one object
[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(thingHappened:)
                                             name:MyThingHappened
                                           object:_myObject];

// Posting notification from another object
[[NSNotificationCenter defaultCenter] postNotificationName:MyThingHappened
                                                    object:self
                                                  userInfo:nil];
```

In this example one object is registering itself as an observer for the MyThingHappened notification, limiting it to when that notification occurs on the exact object, `_myObject`. Then, another object is posting that notification with itself as the object and nothing as the metadata dictionary, `userInfo`. So in this case, if the object that's posting the notification is `_myObject` from the context of the registering object, then the notification will cause thingHappened: to be called.

## Why and When to Use Notification?
### Coupling
Notifications result in loose coupling between objects. The coupling is **loose** because the object sending a notification doesn't know what is listening to the notification. This loose coupling can be extremely powerful because multiple objects can all register to listen to the same notification, from different parts of an application. However, loose coupling can also make the use of notifications a nightmare to debug, since code can easily become entangled. Following the code paths produced when a notification is broadcast can become a daunting experience.
### Multiple Listeners
By design, notifications offer multiple listeners. There can be zero, one or many listeners for any one notification. If you are certain that you'll need multiple listeners, a notification is the correct route to go down. The instance above of login state handling is a perfect example of this. It's likely that multiple objects are going to care about login state changing, so a notification makes sense.
### Is the Object Known?
Sometimes you don't actually know the precise object you want to communicate with. An example of such a scenario is the keyboard in iOS, which is a view which is handled entirely by iOS for you.

## Example in my project - TimePocket
In my project 'TimePocket', I use notification to pass `managedObjectContext` from `AppDelegate` to the `ViewController` at a proper time during the life cycle.

### 1. Create a Header File - Preparing for the Notification
Create a header file called `EventDatabaseAvailability.h`
In the file add the following two constants:

```objc
#define EventDatabaseAvailabilityNotification @"EventDatabaseAvailabilityNotification"
#define EventDatabaseAvailabilityContext @"Context"
```
### 2. Post Notification
When the initialization of my database is completed, I post my notification there in order to pass the `managedObjectContext`.

Post notification code:

```objc
NSDictionary *userInfo = self.eventDatabaseContext ? @{ EventDatabaseAvailabilityContext : self.eventDatabaseContext } : nil;
        [[NSNotificationCenter defaultCenter] postNotificationName:EventDatabaseAvailabilityNotification object:self userInfo:userInfo];
```
### 3. Listen to the Notification
In my `ViewController`, the notification observer should be added as early as possible in its life cycle. Therefore, it's added in `awakeFromNib`, which is the earliest part in the controller's life cycle.

Add the notification observer in `- (void)awakeFromNib` to listen to the notification.

```objc
- (void)awakeFromNib {
    [super awakeFromNib];
    // KF 11/28/2016
    // Listen to the Notification from AppDelegate
    [[NSNotificationCenter defaultCenter]
     addObserverForName:EventDatabaseAvailabilityNotification
     object:nil
     queue:nil
     usingBlock:^(NSNotification * _Nonnull note) {
         self.managedObjectContext = note.userInfo[EventDatabaseAvailabilityContext];
     }];
}
```

<br><br>
***KF***