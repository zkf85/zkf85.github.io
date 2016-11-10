---
layout: post
title: "Class Renaming Issue"
category: [iOS Development]
tag: [ios, objective-c, xcode, technical problem]
---
<div class = "message">
This is a common issue that anybody using XCode may encounter. It's usually very annoying when you want to change the names of the classes because file names and includes are all related when you do it. Here I gather some correct (should be) way to make it happen. 
</div>

## 1. Change Default View Controller Name

Check this: [How to rename default view controllers in XCode](http://rshankar.com/how-to-rename-default-view-controllers-in-xcode/)
In short, it looks like this:

- **Step 1:** Navigate to FirstViewController.h and select the name after interface keyword.

- **Step 2:** Click the Edit menu then Refactor and select Rename from the list of available option.

- **Step 3:** Enter the name then select Rename related files and click the Preview button. This would display the list of files and location where the rename would affect.

- **Step 4:** If you are happy with Preview then save the changes.

I tried the steps above and it works fine.

According to personal experience, you'd better change your default controller name right after you create your new  application. As more files and classes are added, there might be more problems emerge.

Oh, by the way, DON'T FORGET to change your controller class name to the new one in the identity inspector in storyboard.

## 2. Change Default AppDelegate Name

I applied the same steps to rename my default `AppDelegate.h/AppDelegate.m` class. It works fine, but you need to change the app delegate include name in `main.m` manually, or there'll be an error.





<br>
***KF***