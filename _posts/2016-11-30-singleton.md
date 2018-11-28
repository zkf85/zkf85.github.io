---
layout: post
title: "Singleton in iOS"
date: "2016-11-30 14:53:00"
category: [iOS Development]
tags: [ios, objective-c, singleton]
---

>What is the concept of singleton in iOS development?

<!--more-->

## What is Singleton
A singleton object provides a global point of access to the resources of its class. Singletons are used in situations where this single point of control is desirable, such as with classes that offer some general service or resource. You obtain the global instance from a singleton class through a factory method.

## Why and When We Use It?
You obtain the global instance from a singleton class through a factory method. The class lazily creates its sole instance the first time it is requested and thereafter ensures that no other instance can be created.[^apple] It’s an extremely powerful way to share data between different parts of code without having to pass the data around manually.[^matt]

Singleton classes are an important concept to understand because they exhibit an extremely useful design pattern. This idea is used throughout the iPhone SDK, for example, UIApplication has a method called sharedApplication which when called from anywhere will return the UIApplication instance which relates to the currently running application.[^matt]


## How to Implement?[^matt]
You can implement a singleton class in Objective-C using the following code:

in `MyManager.h`:

```objc
#import <foundation/Foundation.h>

@interface MyManager : NSObject {
    NSString *someProperty;
}

@property (nonatomic, retain) NSString *someProperty;

+ (id)sharedManager;

@end
```

in `MyManager.m`:

```objc
#import "MyManager.h"

@implementation MyManager

@synthesize someProperty;

#pragma mark Singleton Methods

+ (id)sharedManager {
    static MyManager *sharedMyManager = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedMyManager = [[self alloc] init];
    });
    return sharedMyManager;
}

- (id)init {
  if (self = [super init]) {
      someProperty = [[NSString alloc] initWithString:@"Default Property Value"];
  }
  return self;
}

- (void)dealloc {
  // Should never be called, but just here for clarity really.
}

@end
```

What this does is it defines a static variable (but only global to this [translation unit](http://en.wikipedia.org/wiki/Translation_unit_(programming))) called `sharedMyManager` which is then initialized once and only once in `sharedManager`. The way we ensure that it’s only created once is by using the `dispatch_once` method from [Grand Central Dispatch (GCD)](http://developer.apple.com/library/ios/#documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html). This is thread safe and handled entirely by the OS for you so that you don’t have to worry about it at all.

However, if you would rather not use GCD then you should use the following code for sharedManager:

Non-GCD based code:

```objc
+ (id)sharedManager {
    static MyManager *sharedMyManager = nil;
    @synchronized(self) {
        if (sharedMyManager == nil)
            sharedMyManager = [[self alloc] init];
    }
    return sharedMyManager;
}
```

Then you can reference the singleton from anywhere by calling the following function:

```objc
MyManager *sharedManager = [MyManager sharedManager];
```

I’ve used this extensively throughout my code for things such as creating a singleton to handle CoreLocation or CoreData functions.


<br><br>
***KF***

### Reference
[^apple]: [Singleton - Apple Developer](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Singleton.html)
[^matt]: [Singletons in Objective-C - Matt Galloway](http://www.galloway.me.uk/tutorials/singleton-classes/)
