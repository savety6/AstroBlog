---
author: Vladimir Kutsev
pubDatetime: 2025-02-25T15:50:00Z
modDatetime: 2025-02-25T09:12:47.400Z
title: First Exercise
slug: first-exercise
featured: false
image: /assets/images/cryBabyMongoDev.webp
ogImage: /assets/images/cryBabyMongoDev.webp
tags:
  - mongodb
  - tutorial
  - exercise
description: First exercise for the MongoDB course.
---

![MongoDB on Docker](/assets/images/cryBabyMongoDev.webp)

MongoDB Shell Crash Course
====================

Table of Contents
-----------------

*   [Introduction](#introduction)
*   [Installation](#installation)
*   [Understanding MongoDB](#understanding-mongodb)
*   [Basic CRUD Operations](#basic-crud-operations)
*   [Indexes](#indexes)
*   [Aggregation](#aggregation)
*   [Conclusion](#conclusion)

Introduction
------------

MongoDB is a popular NoSQL database that stores data in flexible, JSON-like documents. This structure allows for dynamic schemas, making it a preferred choice for modern applications that require scalability and performance.

### Prerequisites

*   **Node.js**: Ensure you have Node.js installed on your system. You can download it from the [official website](https://nodejs.org/).

1.  **Start the MongoDB Server**: Run the following command to start the MongoDB server:
    
    ```bash
    mongod
    ```
    
    By default, MongoDB listens on port 27017.
    
2.  **Access the MongoDB Shell**: Open a new terminal window and execute:
    
    ```bash
    mongosh
    ```
    
    This command opens the MongoDB shell, allowing you to interact with your databases.
    

Understanding MongoDB
---------------------

### Databases and Collections

*   **Database**: A container for collections. Each database gets its own set of files on the file system.
*   **Collection**: A group of MongoDB documents, akin to a table in relational databases.

### Documents

Documents are the basic units of data in MongoDB, stored in a format called BSON (Binary JSON). An example document:

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "John Doe",
  "age": 29,
  "email": "johndoe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  }
}
```

Basic CRUD Operations
---------------------

### 1\. Create (Insert)

To add a new document to a collection:

```javascript
db.users.insertOne({
  "name": "Jane Smith",
  "age": 32,
  "email": "janesmith@example.com"
});
```

### 2\. Read (Find)

Retrieve documents from a collection:

```javascript
// Find all documents
db.users.find();

// Find documents with a specific condition
db.users.find({ "age": { $gt: 30 } });
```

### 3\. Update

Modify existing documents:

```javascript
// Update a single document
db.users.updateOne(
  { "name": "Jane Smith" },
  { $set: { "age": 33 } }
);

// Update multiple documents
db.users.updateMany(
  { "age": { $lt: 25 } },
  { $set: { "status": "Underage" } }
);
```

### 4\. Delete

Remove documents from a collection:

```javascript
// Delete a single document
db.users.deleteOne({ "name": "Jane Smith" });

// Delete multiple documents
db.users.deleteMany({ "status": "Underage" });
```

Indexes
-------

Indexes support the efficient execution of queries. Without indexes, MongoDB must perform a collection scan, i.e., scan every document in a collection, to select those documents that match the query statement.

### Creating an Index

```javascript
// Create an index on the 'email' field
db.users.createIndex({ "email": 1 });
```

### Viewing Indexes

```javascript
// List all indexes on the 'users' collection
db.users.getIndexes();
```

Aggregation
-----------

Aggregation operations process data records and return computed results. They are useful for operations such as grouping values from multiple documents or performing operations on grouped data.

### Example: Aggregation Pipeline

```javascript
db.orders.aggregate([
  { $match: { "status": "shipped" } },
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } }
]);
```

