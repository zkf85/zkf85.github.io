---
layout: post
title: "Collection View I - Intro & Basics"
date: "2016-11-24 16:00:00"
category: [iOS Development]
tags: [ios, uicollectionview, objective-c]
---
<div class = "message">
"Collection View is super powerful." This article includes notes on reading Apple Guide Document " Collection View Programming Guide for iOS".
<br>KF 11/24/2016
</div>

# About iOS Collection Views
- A collection view is a way to present an ordered set of data items using a flexible and changeable layout.

- Collection views keep a strict separation between the data being presented and the visual elements used to present that data.

- In most cases, your app is solely responsible for managing the data. Your app also provides the view objects used to present that data. 

- The collection view is all about the presentation and arrangement of your views and not about their content.

- The **Flow Layout** Supports Grids and Other Line-Oriented Presentations
<!--more-->

# Collection View Basics
 - your app must provide a data source object that tells the collection view how many items there are to display. 

## The Classes and Protocols for Implementing Collection Views

**Table 1-1** The classes and protocols for implementing collection views

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

**Figure 1-1** shows the relationship between the core objects associated with a collection view. The collection view gets information about the cells to display from its data source. The data source and delegate objects are custom objects provided by your app and used to manage the content, including the selection and highlighting of cells. The layout object is responsible for deciding where those cells belong and for sending that information to the collection view in the form of one or more layout attribute objects. The collection view then merges the layout information with the actual cells (and other views) to create the final visual presentation.


#### <center> Figure 1-1. Merging content and layout to create the final presentation </center>
![figure-1-1](/public/img/20161124-1-1.png)


When creating a collection view interface, you first add a UICollectionView object to your storyboard or nib file. Think of the collection view as the central hub, from which all other objects emanate. After adding that object, you can begin to configure any related objects, such as the data source or delegate. All configurations are centered around the collection view itself. For example, you never create a layout object without also creating a collection view object.

## Reusable Views Improve Performance
Collection views employ a view recycling program to improve efficiency. As views move offscreen, they are removed from view and placed in a **reuse queue** instead of being deleted. As new content is scrolled onscreen, views are removed from the queue and repurposed with new content. To facilitate this recycling and reuse, all views displayed by the collection view must descend from the `UICollectionReusableView` class.

Collection views support three distinct types of reusable views, each of which has a specific intended usage:

- *Cells* present the main content of your collection view. The job of a cell is to present the content for a single item from your data source object. Each cell must be an instance of the `UICollectionViewCell` class, which you may subclass as needed to present your content. Cell objects provide inherent support for managing their own selection and highlight state. To actually apply a highlight to a cell, you must write some custom code. For information on implementing cell highlighting/selecting, see Managing the Visual State for Selections and Highlights.
- *Supplementary* views display information about a section. Like cells, supplementary views are data driven. Unlike cells, supplementary views are not mandatory, and their usage and placement is controlled by the layout object being used. For example, the flow layout supports **headers** and **footers** as optional supplementary views.
- *Decoration* views are visual adornments that are wholly owned by the layout object and are not tied to any data in your data source object. For example, a layout object might use decoration views to implement a custom background appearance.

Unlike table views, collection views impose no specific style on the cells and supplementary views provided by your data source. Instead, the basic reusable view classes are blank canvases for you to modify. 

## The Layout Object Controls the Visual Presentation
The layout object is solely responsible for determining the placement and visual styling of items within the collection view. Although your data source object provides the views and the actual content, the layout object determines the size, location, and other appearance-related attributes of those views. This separation of responsibilities makes it possible to change layouts dynamically without changing any of the data objects managed by your app.

The layout process used by collection views is related to, but distinct from, the layout process used by the rest of your app’s views. A layout object never touches the views it manages directly because it does not actually own any of those views. Instead, it generates attributes that describe the location, size, and visual appearance of the cells, supplementary views, and decoration views in the collection view. It is then the job of the collection view to apply those attributes to the actual view objects.

**Figure 1-2** shows how a vertically scrolling flow layout arranges its cells and supplementary views. In a vertically scrolling flow layout, the width of the content area remains fixed and the height grows to accommodate the content. To compute the area, the layout object places views and cells one at a time, choosing the most appropriate location for each. In the case of the flow layout, the size of the cells and supplementary views are specified as properties, either on the layout object or by using a delegate. Computing the layout is just a matter of using those properties to place each view.

#### <center>Figure 1-2  The layout object provides layout metrics</center>
![figure-1-2](/public/img/20161128-1-1.png)

Layout objects control more than just the size and position of their views. The layout object can specify other view-related attributes, such as its **transparency**, its **transform in 3D space**, and its **visibility** (if any) above or below other views. These attributes let you create more interesting layouts. For example, you might create stacks of cells by placing the views on top of one another and changing their z-ordering, or you might use a transform to rotate them on any axis.

<br><br>
***KF***