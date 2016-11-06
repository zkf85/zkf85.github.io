--- 
layout: post
title: "iOS7 CS193P 13/14 Photomania Demo Note"
category: iOS
tag: [ios, core data, multitask, UITableView, delegate]
---
<div class = "message">
This might be the toughest and the biggest demo in this iOS course. Here I put my note collection here as reference for later use.
</div>

# I. Create the Project
- Create a project with single view as normal.

# II. Database Setup

## 1. Create the Database
Create a database file. Then, add the following two entities into the database:
	 
- **Photo**
- **Photographer**

## 2. Define the Schema [^schema] visually.

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



---
[^schema]: A **schema** is a collection of database objects (as far as this hour is concerned—tables) associated with one particular database username. This username is called the schema owner, or the owner of the related group of objects. You may have one or multiple schemas in a database. Jul 1, 2008 [Managing Database Objects in SQL](http://www.informit.com/articles/article.aspx?p=1216889&seqNum=2)