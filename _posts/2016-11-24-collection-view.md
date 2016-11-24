---
layout: post
title: "Collection View"
category: [iOS]
tags: [ios, uicollectionview, objective-c]
---
<div class = "message">
"Collection View is super powerful."
<br>KF 11/24/2016
</div>

## About iOS Collection Views
- A collection view is a way to present an ordered set of data items using a flexible and changeable layout.

- Collection views keep a strict separation between the data being presented and the visual elements used to present that data.

- In most cases, your app is solely responsible for managing the data. Your app also provides the view objects used to present that data. 

- The collection view is all about the presentation and arrangement of your views and not about their content.

- The **Flow Layout** Supports Grids and Other Line-Oriented Presentations

## Collection View Basics
 - your app must provide a data source object that tells the collection view how many items there are to display. 

### The Classes and Protocols for Implementing Collection Views

 Purpose | Classes/Protocols 
:------- |:-----------------
Top-level containment <br>and management| `UICollectionView`<br>`UICollectionViewController`
Content <br>management | `UICollectionViewDataSource` protocol<br>`UICollectionViewDelegate` protocol
Presentation | `UICollectionReusableView`<br>`UICollectionViewCell`
Layout 		| `UICollectionViewLayout`<br>`UICollectionViewLayoutAttributes`<br> `UICollectionViewUpdateItem`
Flow Layout 	| `UICollectionViewFlowLayout`<br>`UICollectionViewDelegateFlowLayout`

### `UICollectionView`
> A `UICollectionView` object defines the visible area for your collection view’s content. This class also facilitates the presentation of your data based on the layout information it receives from its layout object.

### `UICollectionViewController`	
> A `UICollectionViewController` object provides view controller–level management support for a collection view. Its use is optional.

### `UICollectionViewDataSource` protocol
> The data source object is the most important object associated with the collection view and is one that you must provide. The data source manages the content of the collection view and creates the views needed to present that content. To implement a data source object, you must create an object that conforms to the `UICollectionViewDataSource` protocol.

### `UICollectionViewDelegate` protocol
> The collection view delegate object lets you intercept interesting messages from the collection view and customize the view’s behavior. For example, you use a delegate object to track the selection and highlighting of items in the collection view. Unlike the data source object, the delegate object is optional.

### `UICollectionReusableView` & `UICollectionViewCell`
> All views displayed in a collection view must be instances of the `UICollectionReusableView` class. This class supports a recycling mechanism in use by collection views. Recycling views (instead of creating new ones) improves performance in general and especially improves it during scrolling. 

> A `UICollectionViewCell` object is a specific type of reusable view that you use for your main data items.

### `UICollectionViewLayout`, `UICollectionViewLayoutAttributes`, `UICollectionViewUpdateItem`
> Subclasses of `UICollectionViewLayout` are referred to as layout objects and are responsible for defining the location, size, and visual attributes of the cells and reusable views inside a collection view.

> During the layout process, a layout object creates layout attribute objects (instances of the `UICollectionViewLayoutAttributes` class) that tell the collection view where and how to display cells and reusable views.

> The layout object receives instances of the `UICollectionViewUpdateItem` class whenever data items are inserted, deleted, or moved within the collection view. You never need to create instances of this class yourself.

### `UICollectionViewFlowLayout` & `UICollectionViewDelegateFlowLayout` protocol
> The `UICollectionViewFlowLayout` class is a concrete layout object that you use to implement grids or other line-based layouts. You can use the class as-is or in conjunction with the flow delegate object, which allows you to customize the layout information dynamically.

Figure 1-1 shows the relationship between the core objects associated with a collection view. The collection view gets information about the cells to display from its data source. The data source and delegate objects are custom objects provided by your app and used to manage the content, including the selection and highlighting of cells. The layout object is responsible for deciding where those cells belong and for sending that information to the collection view in the form of one or more layout attribute objects. The collection view then merges the layout information with the actual cells (and other views) to create the final visual presentation.

![figure-1-1](/public/img/20161124-1-1.png)

<br><br>
***KF***