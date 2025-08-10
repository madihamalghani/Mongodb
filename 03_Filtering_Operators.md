# Filtering Operators:

Used to filter data by find method.

## *Comparison Operators (with Examples)*

| Operator | Meaning | Example | Explanation |
| --- | --- | --- | --- |
| `$eq` | Equals | `{ age: { $eq: 18 } }` | Find documents where age is **equal to 18** |
| `$ne` | Not equals | `{ age: { $ne: 18 } }` | Find where age is **not equal to 18** |
| `$gt` | Greater than | `{ age: { $gt: 18 } }` | Find where age is **greater than 18** |
| `$gte` | Greater than or equal | `{ age: { $gte: 18 } }` | Find where age is **18 or more** |
| `$lt` | Less than | `{ age: { $lt: 18 } }` | Find where age is **less than 18** |
| `$lte` | Less than or equal | `{ age: { $lte: 18 } }` | Find where age is **18 or less** |
| `$in` | In list of values | `{ name: { $in: ["Ali", "Ayesha"] } }` | Matches any name in the list |
| `$nin` | Not in list | `{ name: { $nin: ["Ali", "Ayesha"] } }` | Excludes names in the list |

---

## Example Queries Using Comparison Operators

### 1. Find all students with marks **greater than 80**:

```notion

db.students.find({ marks: { $gt: 80 } })

```

### 2. Find users **not equal to age 25**:

```notion
db.users.find({ age: { $ne: 25 } })

```

### 3. Find products **priced between 100 and 500**:

```notion

db.products.find({ price: { $gte: 100, $lte: 500 } })

```

### 4. Find names **in a list**:

```notion

db.students.find({ name: { $in: ["Sara", "Ali", "Ahmed"] } })

```

---

## Tip:

You always use comparison operators **inside curly braces `{}`** and **with a field name**.

---

## *MongoDB Logical Operators (with Simple Examples)*

Logical operators allow you to **combine multiple conditions** inside a query — like "this AND that", or "this OR that".

---

### 1. `$and` – All conditions must be true

```notion
db.students.find({
  $and: [
    { age: { $gte: 18 } },
    { grade: "A" }
  ]
})
```

 **Explanation**: Finds students who are **18 or older AND have grade A**

---

### 2. `$or` – At least one condition must be true

```notion

db.students.find({
  $or: [
    { age: { $lt: 18 } },
    { grade: "F" }
  ]
})

```

**Explanation**: Finds students who are **under 18 OR have grade F**

---

### 3. `$not` – Negates a condition

```notion

db.students.find({
  age: { $not: { $gt: 18 } }
})

```

**Explanation**: Finds students **NOT older than 18** (so, 18 or less)

---

### 4. `$nor` – None of the conditions should be true

```notion

db.students.find({
  $nor: [
    { grade: "A" },
    { grade: "B" }
  ]
})

```

**Explanation**: Finds students who have **neither grade A nor B**

---

## Tip:

You can also **combine logical and comparison operators** together. For example:

```notion

db.students.find({
  $and: [
    { age: { $gt: 15 } },
    { marks: { $lte: 80 } }
  ]
})

```

---

## Summary Table

| Operator | Description | Example Use |
| --- | --- | --- |
| `$and` | All conditions must be true | Combine filters |
| `$or` | Any condition is true | Multiple options |
| `$not` | Negates a condition | Inverts a check |
| `$nor` | All conditions must be false | Opposite of `$or` |

---

## *What Are Element Operators?*

> Element Operators are used to check whether a field exists or not, and to check the type of the value in a field.
> 

There are **two main element operators**:

| Operator | Meaning |
| --- | --- |
| `$exists` | Checks if a field **exists** in the document |
| `$type` | Checks the **data type** of a field |

---

## 1. `$exists`

### Check if a field is present

```notion

db.users.find({ email: { $exists: true } })

```

🔸 **Finds users who have an `email` field** (it could be empty, but the field must exist)

---

### Check if a field is missing

```notion

db.users.find({ phone: { $exists: false } })

```

🔸 **Finds users who do NOT have a `phone` field**

---

## 2. `$type`

### Check the data type of a field

```notion

db.users.find({ age: { $type: "int" } })

```

🔸 **Finds users where `age` is stored as an integer**

---

### You can use type numbers too:

```notion

db.logs.find({ createdAt: { $type: 9 } }) // 9 = date

```

---

### Common Type Codes

| Type | Code | Example |
| --- | --- | --- |
| Double | 1 | `4.5` |
| String | 2 | `"Hello"` |
| Object | 3 | `{}` |
| Array | 4 | `[1, 2, 3]` |
| Boolean | 8 | `true/false` |
| Date | 9 | `new Date()` |
| Null | 10 | `null` |
| Int | 16 | `25` |

---

## Real-world Example

### Find users whose `salary` field exists and is a number:

```notion

db.employees.find({
  salary: { $exists: true, $type: "number" }
})

```

---

## Table:

| Operator | What it does | Example |
| --- | --- | --- |
| `$exists` | Check if a field exists | `{ field: { $exists: true } }` |
| `$type` | Check data type of a field | `{ field: { $type: "string" } }` |

---

## Array Operators:

In MongoDB, **array operators** allow you to query and update documents based on the contents of arrays.

---

### **Common Array Operators in MongoDB**

---

### **1. `$all`**

Matches arrays that contain **all** specified elements (regardless of order).

```jsx

db.courses.find({ tags: { $all: ["math", "science"] } })

```

> Finds documents where tags contains both "math" and "science".
> 

---

### **2. `$elemMatch`**

Matches **at least one element** of the array that matches **multiple conditions**.

```jsx

db.students.find({
  scores: { $elemMatch: { subject: "Math", marks: { $gt: 90 } } }
})

```

> Use when you want to apply multiple conditions to a single array element.
> 

---

### **3. `$size`**

Matches arrays with a **specific number of elements**.

```jsx

db.books.find({ authors: { $size: 2 } })

```

> Finds books with exactly 2 authors.
> 

---

### **4. `$in` (with arrays)**

Matches if **any** array element matches one of the values.

```jsx

db.movies.find({ genres: { $in: ["Action", "Comedy"] } })

```

> Returns documents where the genres array contains either "Action" or "Comedy".
> 

---

### Example Document:

```json

{
  name: "Ali",
  hobbies: ["reading", "coding", "swimming"],
  scores: [
    { subject: "Math", marks: 92 },
    { subject: "English", marks: 85 }
  ]
}

```

---

## Projection:

> Projection in MongoDB is used to select specific fields to include or exclude in the result of a query.
> 

---

### Why use projections?

- To limit data sent from the database.
- To **improve performance** by avoiding unnecessary fields.
- To **customize output** (e.g., only get names and emails, not passwords).

---

### Basic Syntax:

```jsx

db.collection.find(query, projection)

```

---

### Examples:

### 1. Include specific fields:

```jsx

db.users.find({}, { name: 1, email: 1 })

```

This will return only `name`, `email`, and `_id` (by default MongoDB includes `_id`).

### 2. Exclude specific fields:

```jsx

db.users.find({}, { password: 0, age: 0 })

```

This will return **everything except** `password` and `age`.

### 3. Exclude `_id` too:

```jsx

db.users.find({}, { name: 1, _id: 0 })

```

---

### 🚫 Mixing include & exclude?

You **cannot** mix inclusion and exclusion in the same projection (except `_id`):

🥂Allowed:

```jsx
{ name: 1, email: 1, _id: 0 }

```

❌ Not allowed:

```jsx

{ name: 1, password: 0 }

```

---

### Table:

| Projection Type | Syntax Example | Result |
| --- | --- | --- |
| Include fields | `{ name: 1, email: 1 }` | Shows only `name` and `email` |
| Exclude fields | `{ password: 0, _id: 0 }` | Hides `password` and `_id` |
| Exclude `_id` | `{ name: 1, _id: 0 }` | Shows `name` only, not `_id` |

---

## Sorting:

> Sorting in MongoDB means arranging the query results in ascending or descending order based on one or more fields.
> 

---

### Syntax:

```jsx

db.collection.find().sort({ fieldName: 1 })   // Ascending
db.collection.find().sort({ fieldName: -1 })  // Descending

```

---

### Examples:

### 1. Sort by name **(A–Z)**:

```jsx

db.users.find().sort({ name: 1 })

```

### 2. Sort by age **(highest to lowest)**:

```jsx
js
CopyEdit
db.users.find().sort({ age: -1 })

```

###  3. Sort by multiple fields:

```jsx

db.users.find().sort({ age: 1, name: -1 })

```

👉 First sorts by age (ascending), then name (descending) if ages are the same.

---

### Notes:

- Sorting is **done after filtering** but **before limiting** results.
- It can **affect performance** if not supported by an index.

---

### With filter and limit:

```jsx

db.users.find({ city: "Lahore" }).sort({ age: -1 }).limit(5)

```

This gives **top 5 oldest users** from Lahore.

## Skip and Limit

These are used for **pagination** and **controlling** the number of results.

---

> Limits the number of documents returned.
> 

**Syntax**:

```jsx

db.collection.find().limit(number)

```

**Example**:

```jsx

db.users.find().limit(5)

```

👉 Returns the **first 5 documents**.

---

### `skip()`

> Skips the given number of documents from the beginning of the result.
> 

**Syntax**:

```jsx

db.collection.find().skip(number)

```

**Example**:

```jsx
js
CopyEdit
db.users.find().skip(5)

```

👉 Skips the **first 5 documents** and shows the rest.

---

### Combine for Pagination:

```jsx

db.users.find().skip(10).limit(5)

```

👉 Skips the **first 10** results and then **returns 5** documents.

Used for showing **page 3** (assuming 5 results per page).

---

### Real Example:

Imagine you have 100 users and want to show 10 users per page.

To get **page 4**, you would:

```jsx
db.users.find().skip(30).limit(10)

```

💡 `skip = (pageNumber - 1) * itemsPerPage`
