# CRUD:

<aside>

- create
- read
- update
- delete
</aside>

### **Create:**

**Create** operation means **inserting new data** (a document) into a collection in MongoDB.

**In Simple Words:**

You're adding a **new record** (called a **document**) into a **table** (called a **collection**).

### Syntax (using MongoDB shell or Node.js driver):

### 👉 Basic Insert One:

```jsx

db.students.insertOne({
  name: "Ayesha",
  age: 20,
  course: "MongoDB"
})

```

### 👉 Insert Many:

```jsx

db.students.insertMany([
  { name: "Ali", age: 21, course: "Node.js" },
  { name: "Sara", age: 22, course: "React" }
])

```

---

### 💡 What happens behind the scenes?

- MongoDB **automatically adds `_id`** (a unique ID) to each document.
- You can also manually give your own `_id`.

```jsx

{
  _id: ObjectId("abc123..."), // auto-generated
  name: "Ayesha",
  age: 20
}

```

---

### Notes:

- Each document is like a **JSON object**.
- Fields don't need to follow a fixed schema — you can have different fields in each document.
- The collection is **created automatically** when you insert the first document.

---

### Example in Node.js (Mongoose):

```jsx

const student = new Student({
  name: "Hassan",
  age: 19,
  course: "Express"
});

await student.save(); // This creates (inserts) the student

```

## Read:

**Read** means fetching or retrieving data (documents) from a MongoDB collection.

- You’re **finding** and **viewing** documents that already exist in the database.

### Most Common Methods:

### `find()` – returns **all matching documents**:

```jsx
db.students.find({ course: "MongoDB" })

```

 Returns an array of documents where `course` is `"MongoDB"`.

### `findOne()` – returns **only the first matching document**:

```jsx

db.students.findOne({ name: "Ayesha" })

```

### 👉 Empty `find()` to get everything:

```jsx
db.students.find({})
```

---

### 💡 Filtering with Queries:

You can use **conditions** just like SQL `WHERE`:

```jsx

db.students.find({ age: { $gt: 20 } }) // age greater than 20
```

Operators:

- `$gt` → greater than
- `$lt` → less than
- `$eq` → equal to
- `$in` → in array

Example:

```jsx

db.students.find({ course: { $in: ["MongoDB", "Node.js"] } })
```

---

### Projecting Specific Fields:

Return only selected fields:

```jsx
db.students.find({}, { name: 1, course: 1, _id: 0 })
```

This shows only `name` and `course` fields.

---

### In Node.js using Mongoose:

```jsx
const students = await Student.find({ course: "MongoDB" });
```

```jsx

const oneStudent = await Student.findOne({ name: "Ayesha" });
```

---

### Pro Tip:

MongoDB reads are **very fast**, especially when using **indexes** on frequently queried fields.

## Update:

**Update** means modifying existing documents in a collection.

### Common Update Methods:

### 👉 `updateOne()`

Updates **only the first** document that matches the filter.

```jsx
db.students.updateOne(
  { name: "Ayesha" },           // Filter
  { $set: { course: "Node.js" } } // Update
)

```

### 👉 `updateMany()`

Updates **all** documents that match the filter.

```jsx

db.students.updateMany(
  { age: { $lt: 20 } },
  { $set: { status: "Minor" } }
)

```

### 👉 `replaceOne()`

Replaces the entire document.

```jsx

db.students.replaceOne(
  { name: "Ayesha" },
  { name: "Ayesha", age: 22, course: "React" } // replaces old doc completely
)

```

---

### Update Operators:

- `$set`: Set a new value for a field
- `$inc`: Increment a numeric field
- `$unset`: Remove a field
- `$rename`: Rename a field

**Examples:**

```jsx
db.students.updateOne(
  { name: "Ali" },
  { $inc: { age: 1 } }  // increases age by 1
)

```

```jsx

db.students.updateOne(
  { name: "Ali" },
  { $unset: { oldField: "" } }
)

```

---

### In Mongoose (Node.js):

```jsx

await Student.updateOne({ name: "Ali" }, { $set: { age: 23 } });

```

```jsx
await Student.findOneAndUpdate(
  { name: "Ali" },
  { $inc: { age: 1 } },
  { new: true }
);

```

---

### ⚠️ Important:

- Always double-check your filter to avoid accidental updates to many documents.
- Use `updateOne` if you're sure it's only one doc.
- Use `updateMany` if updating multiple.

## Delete:

**Delete** means removing documents from a collection.

### Common Delete Methods:

### 👉 `deleteOne()`

Deletes the **first** document that matches the filter.

```jsx

db.students.deleteOne({ name: "Ali" })

```

> Only the first matching document is deleted, even if multiple match.
> 

---

### 👉 `deleteMany()`

Deletes **all documents** that match the filter.

```jsx

db.students.deleteMany({ course: "Math" })

```

> ⚠️ Be careful! This deletes all matching documents.
> 

---

### ❌ Delete Everything (Use with caution!)

```jsx

db.students.deleteMany({})

```

> ⚠️ This deletes all documents in the collection but not the collection itself.
> 

---

### In Mongoose (Node.js):

```jsx
// Delete one
await Student.deleteOne({ name: "Ali" });

// Delete many
await Student.deleteMany({ course: "Math" });

// Find and delete
await Student.findOneAndDelete({ name: "Ayesha" });

```

---

### 🛡️ Best Practices:

- Always **use filters carefully** to avoid deleting unintended documents.
- Consider using `.find()` before `.deleteOne()` if unsure.
