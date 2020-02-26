# MongoDB Commands

## 1️⃣ Introduction

---

MongoDb is a NoSQL database which means : it doesn't use SQL at all ! which also means :

- no table _( as SQL )_
- no field _( as SQL )_

MongoDB is using JS (JavaScript)

## 2️⃣ MongoDB logic 'Keys'

---

MongoDB is based on JSON (JavaScript Object Notation) which is a format for storing data

JSON is from JS it is this quite natturally that JSON is **object-oriented**

You should ask yourself :

"How do I store all my data on MongoDB, I don't understand how to create 'tables' and 'fields' ? "

Well, there isn't any table but we speaks about **Collections**, and there is not restricted field like _MySQL or Oracle_ because you are storing 'JSON objects'

To better understand, **_associate Collection to a bag and your objects to glass balls_**. You can store any type of glass balls you want / needs in your bags, they don't needs to be identical.

- Example

```json
{ "name":"John", "age":30, "car":null }
{ "car":"Audi", "model":"TT", "doors": 5, "color": "white" }
```

## 3️⃣ Database

---

- Create Database : `use databaseName`
- Show Database : `show databases`
- Show Collections _(Show Tables)_ : `show collections`
- Use databaseName : `use databaseName` (it will create one if it doesn't exist)

## Collections _(Tables)_

- Create collection (create table) : `db.createcollection("collectionName")`
- Drop collection (drop table): `db.collectionName.drop()`

## 4️⃣ Select

---

- Select \* from collectionName : `db.collectionName.find()`
- Select \* From collectionName Where Level = "info" :

```json
db.collectionName.find({
    "Level": "info"
});

OR
/* this one is better | $eq means equal */
db.Log.find({
   "Level": { $eq: "info" }
});
```

## 5️⃣ Comparaison Query

---

- \$eq : equal | =
- \$gt : greater than | >
- \$gte : greater or equal | >=
- \$in : matches values in an array | in (1,2,3)
- \$lt : lower than | <
- \$lte : lower than or equal | <=
- \$ne : not equal | <>
- \$nin : not int | not in (1,2,3)
- /value/ : like | like '%value%' ( ex : "Name": /Joh/ )

#### Example

```json
/* get all the object where the date is lower than now */
db.Log.find(
    {
        "Date":
        {
            $lt: new Date(new Date().setDate(new Date().getDate()))
        }
    });

```
