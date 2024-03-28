---
title: "Relational Table Design"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## ArtBasket Relational Database Design

## Introduction

This document provides a detailed description of the relational database design for the ArtBasket project. The database is divided into two main sections: the Creator Side and the Consumer Side, each with its own set of tables.

## Creator Side

### User Table

**Description:** Stores information about users who create content on the platform.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each user.|
| `emailID` | VARCHAR | Email address of the user.|
| `username` | VARCHAR | User's chosen username.|
| `password` | VARCHAR | Hashed password for user authentication.|
| `ImageID` | INT | Foreign key linking to the Image table for the user's profile picture.|

### Project Table

**Description:** Holds information about projects created by users.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each project. |
| `userID` | INT | Foreign key linking to the User table to identify the project creator.|
| `Title` | VARCHAR | Title of the project.|
| `Type` | VARCHAR | Type of the project (e.g., comic, story).|
| `Description` | TEXT | Detailed description of the project.|
| `gen_length` | INT | Length of the generated content (e.g., number of pages).|

### Story Table

**Description:** Contains stories created by users.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each story.|
| `Content` | TEXT | The content of the story.|
| `CharacterIDs` | INT ARRAY | Array of foreign keys linking to the Character table.|
| `storyboardID` | INT | Foreign key linking to the StoryBoard table.|

### Character Table

**Description:** Stores details about characters used in stories.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each character.|
| `Summary` | TEXT | A brief summary or description of the character.|
|`Tags` | VARCHAR ARRAY | Array of tags associated with the character.|
| `ImageID` | INT | Foreign key linking to the Image table for the character's image.|

### StoryBoard Table

**Description:** Represents a storyboard, a collection of pages that make up a comic or story.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each storyboard. |

### Page Table

**Description:** Contains information about individual pages within a storyboard.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each page. |
| `Summary` | TEXT | A brief summary of the page content.|
| `Panel_length` | INT | Number of panels on the page.|
| `Layout` | VARCHAR | Description of the page layout.|
| `StoryboardID` | INT | Foreign key linking to the StoryBoard table.|

### Panel Table

**Description:** Stores details about each panel on a page.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each panel.|
|`Description` | TEXT | Description of the panel content.|
| `Tags` | VARCHAR ARRAY | Array of tags associated with the panel.|
| `CharacterID` | INT ARRAY | Array of foreign keys linking to the Character table.|
| `ImageID` | INT | Foreign key linking to the Image table for the panel's image.|
| `PageID` | INT | Foreign key linking to the Page table.|

### Image Table

**Description:** Holds information about images used in the platform.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each image.|
| `URL` | VARCHAR | URL of the image.|

## Consumer Side

### Comment Table

**Description:** Stores comments made by users on various content.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each comment.|
| `userID` | INT | Foreign key linking to the User table to identify the commenter.|
| `text` | TEXT | The content of the comment.|

### Reaction Table

**Description:** Records reactions (likes, dislikes, etc.) by users on content.

|Columns|Datatype|Description|
|--------|-----------------|----------|
| `ID` | INT | Primary key, unique identifier for each reaction.|
| `userID` | INT | Foreign key linking to the User table to identify the reacting user.|
| `type` | VARCHAR | Type of reaction (e.g., like, dislike).|

### Follow Table

**Description:** Keeps track of user follow relationships, where users can follow creators on the platform.

|Columns|Datatype|Description|
|--------|-----------------|----------|
|`ID` | INT | Primary key, unique identifier for each follow relationship.|
| `userID` | INT | Foreign key linking to the User table to identify the follower.|
| `celebID` | INT | Foreign key linking to the User table to identify the followed creator (celebrity).|

## Conclusion

The relational database design for the ArtBasket project is structured to support both the creation and consumption of content. Detailed table schemas ensure that the database is organized, scalable, and capable of supporting future enhancements. Each table is designed with primary and foreign keys to maintain data integrity and facilitate relationships between different entities in the platform.
