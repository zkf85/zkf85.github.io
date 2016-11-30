---
layout: post
title: "Collection View II - Designing Your Data Source and Delegate"
date: "2016-11-28 14:00:00"
category: [iOS Development]
tags: [ios, uicollectionview, objective-c]
---
<div class = "message">
Continue - Collection View learning notes on reading Apple Guide Document " Collection View Programming Guide for iOS".
<br>KF 11/28/2016
</div>
# II.Designing Your Data Source and Delegate
The **data source** object is the content that your app displays. The only requirement of the data source is that it must be able to provide information that the collection view needs, such as
- how many items there are and 
- which views to use when displaying those items.

The **delegate object** is an optional (but recommended) object that manages aspects related to the presentation of and interaction with your content.  Although the delegate’s main job is to manage cell highlighting and selection, it can be extended to provide additional information. For example, the flow layout extends the basic delegate behavior to customize layout metrics, such as the size of cells and the spacing between them.
<!--more-->

## The Data Source Manages Your Content
The data source object is the object responsible for managing the content you are presenting using a collection view. The data source object must conform to the `UICollectionViewDataSource` protocol, which defines the basic behavior and methods that you must support. The job of the data source is to provide the collection view with answers to the following questions:

- How many sections does the collection view contain?
- For a given section, how many items does a section contain?
- For a given section or item, what views should be used to display the corresponding content?

**Sections** and **items** are the fundamental organizing principle for collection view content.
A collection view typically has **at least one section** and may contain more. Each section, in turn, contains **zero** or more items. Items represent the main content you want to present, whereas sections organize those items into logical groups. For example, a photo app might use sections to represent a single album of photos or a set of photos taken on the same day.

The collection view refers to the data it contains using NSIndexPath objects. When trying to locate an item, the collection view uses the index path information provided to it by the layout object. For items, the index path contains a section number and an item number. For supplementary and decoration views, the index path contains whichever values were provided by the layout object. 

> **Note:** Although standard index paths support multiple levels, the collection view’s cells only supports index paths that are 2-levels deep with “section” and “item” parameters, much like the index paths for the UITableView class.

Different layout objects could present section and item data very differently, as shown in **Figure 2-1**.

#### <center>Figure 2-1. Sections arranged according to the arrangement of layout objects</center>
![figure-2-1](/public/img/20161128-2-1.png)

### Designing Your Data Objects
Organizing your data into sections and items makes it much easier to implement your data source methods later. And because your data source methods are called frequently, you want to make sure that your implementations of those methods are able to retrieve data as quickly as possible.

One simple solution (but certainly not the only solution) is for your data source to use a set of nested arrays, as shown in **Figure 2-2**. 

#### <center>Figure 2-2. Arranging data objects using nested arrays</center>

<center><img src="/public/img/20161128-2-2.png" width="5000"/></center>


> **Note:** In general, your data objects should never be a performance bottleneck. The collection view usually accesses your data source only to calculate how many objects there are in total and to obtain views for elements that are currently onscreen. If the layout object relies only on data from your data objects, performance could be severely impacted when the data source contains thousands of objects.

### Telling the Collection View About Your Content
Among the questions asked of your data source by the collection view are how many sections it contains and how many items each section contains. The collection view asks your data source to provide this information when any of the following actions occur:

* The collection view is displayed for the first time.
* You assign a different data source object to the collection view.
* You explicitly call the collection view’s `reloadData` method.
* The collection view delegate executes a block using `performBatchUpdates:completion:` or any of the move, insert, or delete methods.

You provide the number of sections using the `numberOfSectionsInCollectionView:` method, and the number of items in each section using the `collectionView:numberOfItemsInSection:` method. You must implement the `collectionView:numberOfItemsInSection:` method, but if your collection view has only one section, implementing the `numberOfSectionsInCollectionView:` method is optional. Both methods return integer values with the appropriate information.

If you implemented your data source as shown in Figure 2-2, the implementation of your data source methods could be as simple as those shown below:

```objc
- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView*)collectionView {
    // _data is a class member variable that contains one array per section.
    return [_data count];
}
 
- (NSInteger)collectionView:(UICollectionView*)collectionView numberOfItemsInSection:(NSInteger)section {
    NSArray* sectionArray = [_data objectAtIndex:section];
    return [sectionArray count];
}
```

## Configuring Cells and Supplementary Views
Another important task of your data source is to provide the views that the collection view uses to display your content. The collection view does not track your app’s content. It simply takes the views you give it and applies the current layout information to them. Therefore, everything that is displayed by the views is your responsibility.

After your data source reports how many sections and items it manages, the collection view asks the layout object to provide layout attributes for the collection view’s content. At some point, the collection view asks the layout object to provide the list of elements in a specific rectangle (often this is the visible rectangle). The collection view uses that list to ask your data source for the corresponding cells and supplementary views. To provide those cells and supplementary views, your code must do the following:

1. Embed your template cells and views in your storyboard file. (Alternatively, register a class or nib file for each type of supported cell or view.)
2. In your data source, dequeue and configure the appropriate cell or view when asked.



<br><br>
***KF***