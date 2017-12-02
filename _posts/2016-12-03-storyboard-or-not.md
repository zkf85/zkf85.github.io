---
layout: post
title: "Should I Use Storyboards or Not?"
date: "2016-12-03 20:08:00"
category: [iOS Development]
tags: [ios, objective-c, storyboard]
---
<div class = "message">
Storyboards are a quick and easy way to start building a simple app. But should you use Storyboards on more complex apps? Or on a team? What are the pros and cons of using them? Do experienced iOS developers use them? Are any mainstream apps built with Storyboards?
</div>

## I. What are the pros and cons of using Storyboards?[^roadfire]

### Pros:
The biggest benefit of using Storyboards (especially over nibs/xibs) is that **you can see how the screens in your app are related**. When you’re just starting on an existing project (or coming back to an old one), Storyboards give you an easy way to follow the flow through the app and zoom out to get a high level overview of what the app does.

Another benefit to using Storyboards (over creating views programmatically) is that **you get to see what your view will look like at runtime without having to run your app**. You can quickly make a change in Interface Builder and immediately see what it’ll look like – without waiting for Xcode to compile and run. (Nibs/xibs get this benefit, too.)

And a third benefit is that **you can create static table views in Storyboards**. So if you have a Settings screen, for example, with tappable rows of static text, Storyboards allow you to quickly and easily create them – no code required.

<!--more-->

### Cons:
**On teams**, Storyboards are tricky to use for two main reasons:

- **Merge conflicts are painful**: When two people edit the same Storyboard file at the same time and cause a merge conflict, it’s tricky to sort out how to merge the changes. Part of this is because of the format of the Storyboard file (it’s XML) and part of it is because Xcode rearranges elements and makes changes in the Storyboard when you open it – even if you don’t touch anything. And when you do touch something, it’s that much worse.
- **Code reviews are tricky**: If you conduct code reviews on your team, reviewing changes to a Storyboard file is… difficult. Again, the format is XML with lots of hard-to-understand attributes and elements, so reviewing a diff and making constructive comments is just not easy.

And those are just the cons to working on a team. Even if you’re working **on your ow**n, there are some cons to using Storyboards:

- **Confusion when they become complicated**: If you have a relatively large app and you keep adding new view controllers, your Storyboard will eventually become complicated. Those pretty arrows that connect one view controller to the next start overlapping each other or getting hidden under view controllers – and it’s really hard to see what’s happening in the app.
- **Xcode grinds to a halt when opening a large Storyboard**: Again, if you’re working on a relatively large app, Storyboards can be difficult since it takes Xcode a while to load them into the editor. I’m working on a project with about 30 view controllers in a Storyboard, and it takes multiple seconds (around 4-5) to load the Storyboard whenever I want to edit it. (And yes, my MBP is fast.)

Whenever I’ve worked on a team, we’ve agreed to **create separate Storyboards for different flows to avoid the confusion and performance issues of massive Storyboards.** And we’ve always been careful to communicate about which Storyboard we’re working on before we start to make changes so we can avoid having to deal with messy merge conflicts. The code review issue exists, and will continue to exist, until Apple changes the format of the XML in the Storyboard file.

## II. Why I’m Abandoning Storyboards[^gopalkri]
Here’s a list of reasons as to why I'm abandoning the storyboard:

### 1. Diffs
Storyboards can’t be diffed meaningfully. Since they serialize to unreadable XML, it’s more or less impossible to look at a diff of a storyboard and figure out what really changed.

### 2. Merges
Because storyboards can’t be read by human beings, merging or rebasing storyboards across branches is an absolute nightmare. It often results in re-creating the work on one of the branches manually.

### 3. Opening a file modifies it
I’m not sure why this happens, and it doesn’t happen all the time, but I’ve often found that merely opening a Storyboard file modifies it. This is super irritating, because I’m no longer sure about whether or not I made a change to it that matters.

### 4. Code review
A broken storyboard can completely break the app (like say, if you’ve got an ambiguous constraint), so it must be code reviewed like all other code. However, because of the above reasons, it is really hard to do a proper code review.

### 5. Usefulness
One of the goals of using Storyboards is to be able to visualize what the app will look like without actually running it. In reality, though, storyboards don’t really come close. 
![image](http://gopalkri.com/assets/images/2016-05-27-Storyboards-Visualization.png)

## What I’m doing instead
I’m going to be using AutoLayout in code. While NSLayoutConstraint is incredibly verbose, there’re a few open source libraries that appear to make it significantly less painful: SnapKit, EasyPeasy, SwiftAutoLayout, etc. I’m not a big fan of the ASCII art version of NSLayoutConstraint. As awesome as it is, it’s stringly typed, and I try and avoid stringly typed code whenever possible.

## III. iOS User Interfaces: Storyboards vs. NIBs vs. Custom Code[^toptal]

[^toptal]: [iOS User Interfaces: Storyboards vs. NIBs vs. Custom Code](https://www.toptal.com/ios/ios-user-interfaces-storyboards-vs-nibs-vs-custom-code)

### iOS Storyboards
> A classic beginner’s mistake is to create one massive project-wide Storyboard. A Storyboard is a board with a story to tell. It shouldn't be used to mix unrelated stories into one big volume.

As its name implies, a Storyboard is a board with a story to tell. It shouldn’t be used to mix unrelated stories into one big volume. A storyboard should contain view controllers that are logically related to each other—which doesn’t mean every view controller.

For example, it makes sense to use Storyboards when handling:

- A set of views for authentication and registration.
- A multi-step order entry flow.
- A wizard-like (i.e., tutorial) flow.
- A master-detail set of views (e.g., profiles lists, profile details).

Meanwhile, large Storyboards should be avoided, including single app-wide Storyboards (unless the app is relatively simple). Before we go any deeper, let’s see why.

#### When to use Storyboards:
> Storyboards are best used with multiple interconnected view controllers, as their major simplification is in transitioning between view controllers.

#### When Not to Use iOS Storyboards

A few cases:

- The view has a complicated or dynamic layout, best-implemented with code.
- The view is already implemented with NIBs or code.

In those cases, we can either leave the view out of the Storyboard or embed it in a view controller. The former breaks the Storyboard’s visual flow, but doesn’t have any negative functional or development implications. The latter retains this visual flow, but it requires additional development efforts as the view is not integrated into the view controller: it’s just embedded as a component, hence the view controller must interact with the view rather than implementing it.


### iOS Custom Code (Programmatic UIs)

#### Pro: Under the Hood
The greatest advantage of creating an iOS UI programmatically: if you know how to code a user interface, then you know what happens under the hood, whereas the same is not necessarily true of NIBs and Storyboards.

So, mastering the coding of iOS user interfaces gives you more control over and greater awareness of how these pieces fit together, which raises your upper-bound as a developer.

#### When to Use Code

It’s often a good call to use use custom code for iOS user interface design when you have:

- Dynamic layouts.
- Views with effects, such as rounded corners, shadows, etc.
- Any case in which using NIBs and Storyboards is complicated or infeasible.

#### When Not to Use Code

In general, code-made UIs can always be used. They’re rarely a bad idea, so I’d put an here.

Fore complete information, check the references.

<br><br>
***KF***

## Reference
[^roadfire]: [Should you use Storyboards on more complex apps?](http://roadfiresoftware.com/2015/03/the-pros-and-cons-of-using-storyboards/)

[^gopalkri]: [Why I’m Abandoning Storyboards](http://gopalkri.com/2016/05/27/why-im-abandoning-storyboards/)



