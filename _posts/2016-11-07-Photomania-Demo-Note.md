--- 
layout: post
title: "iOS7 CS193P 13/14 Photomania Demo Note"
category: iOS Development
tag: [ios, core data, multitask, uitableview, delegate]
---
<div class = "message">
This might be the toughest and the biggest demo in this iOS course. Here I put my note collection here as reference for later use. 11/06/2016
</div>

# I. Create the Project
- Create a project with single view as normal.

# II. Database Setup

## 1. Create the Database
Create a database file. Then, add the following two entities into the database:
	 
- **Photo**
- **Photographer**

## 2. Define the Schema [^schema] Visually.

- Add properties to them respectively. (e.g. title, url, data, name etc.)

- Add relationship between the two entities:
	- For **Photographer** in **Photo**, the relationship is ***whoTook***
	- For **Photo** in **Photographer** the relationship is the same one, but called ***photos***.

		(**NOTICE**: The relationship type here should be set as ***To Many***, because one photographer may take multiple photos.)
	
## 3. Create NSManagedObject Subclasses
- Create NSManagedObject Subclasses automatically using XCode.
	- Select both entities;
	- click "Editor", then "Create NSManagedObject Subclasses";
	- The correspondent subclasses **Photo.h/Photo.m** and **Photographer.h/Photographer.m** are created in the project and can be found in the project navigator.
	- Notice that in XCode 7 and later, the generated new subclasses include: entity and their CoreDataProperties category like this:

		![subclasses](/public/img/20161106-0.png)
	
		To understand this, check [this Q&A on Stack Overflow](http://stackoverflow.com/questions/33106098/xcode-7-generates-core-data-entity-with-additional-coredataproperties-category). The gist is that you can use  the two files *without* "properties" to add some custom methods or properties. In this way, whenever you update your database property setup and regenerate these supclasses, these custom methods will still be there.

	`continue on 11/07/2016`

	- Also another thing to mention is that don't forget to choose your data model and go to the file inspector, then choose the code generation language to what you desired. Here I use OC. If don't do this, language compatibility issue may occur. 

## 4. Add Custom Methods to the Subclasses
Add these too class methods to `Photos+CoreDataClass`

```objective-c
+ (Photo *)photoWithFlickrInfo:(NSDictionary *)photoDictionary
        inManagedObjectContext:(NSManagedObjectContext *)context;

+ (void)loadPhotosFromFlickrArray:(NSArray *)photos
         intoManagedObjectContext:(NSManagedObjectContext *)context;
```

---
[^schema]: A **schema** is a collection of database objects (as far as this hour is concerned—tables) associated with one particular database username. This username is called the schema owner, or the owner of the related group of objects. You may have one or multiple schemas in a database. Jul 1, 2008 [Managing Database Objects in SQL](http://www.informit.com/articles/article.aspx?p=1216889&seqNum=2)