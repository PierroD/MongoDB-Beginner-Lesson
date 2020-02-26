# Resquest Eloquent to MongoDB

## Request Summary

- [Eloquent To MongoDB](#eloquent-to-mongodb)
- [Aggregate Date](#aggregate-date)
- [Between Two Dates](#between-two-dates)
- [Solution](#solution)

## <a name="eloquent-to-mongodb"></a> 1️⃣ Datetime Eloquent to mongodb

---

- Transform Eloquent to query | example :

```php
   DB::enableQueryLog();
   $logs = Log::where('Date', '>', $request->date);
   $logs->orderBy('Date', 'DESC')->get();
   dd(DB::getQueryLog());
```

- Output

```php
 "query" => "Log.find({"Date":{"$gte":{"$date":{"$numberLong":"1582588800000"}}}},{"sort":{"Date":-1},"typeMap":{"root":"array","document":"array"}})"
    "bindings" => []
    "time" => 2.12
```

## <a name="aggregate-date"></a> 2️⃣ MongoDB to aggregate date format

---

- Request

```jql
db.Log.aggregate(
    {
        $group: {
            _id: {
                year: { $year : "$Date" },
                month: { $month : "$Date" },
                day: { $dayOfMonth : "$Date" },
                hour: { $hour : "$Date" },
                minutes: { $minute: "$Date" },
            	seconds: { $second: "$Date" },
          		milliseconds: { $millisecond: "$Date" },
           		dayOfYear: { $dayOfYear: "$Date" },
           		dayOfWeek: { $dayOfWeek: "$Date" },
           		week: { $week: "$Date" }
            },
            count: { $sum: 1 }
        }
    });

```

- Results :

```jql
/* 1 */
{
	"_id" : {
		"year" : 2020,
		"month" : 2,
		"day" : 24,
		"hour" : 16,
		"minutes" : 21,
		"seconds" : 9,
		"milliseconds" : 409,
		"dayOfYear" : 55,
		"dayOfWeek" : 2,
		"week" : 8
	},
	"count" : 1
},

/* 2 */
{
	"_id" : {
		"year" : 2020,
		"month" : 2,
		"day" : 24,
		"hour" : 14,
		"minutes" : 3,
		"seconds" : 8,
		"milliseconds" : 975,
		"dayOfYear" : 55,
		"dayOfWeek" : 2,
		"week" : 8
	},
	"count" : 1
},
...
```

## <a name="between-two-dates"></a> 3️⃣ Between two Dates

---

- Request

```jql
db.Log.find({
    "Date" : {
        $lt: new Date(),
        $gte: new Date(`2020-02-25`)
    }});
```

- Output

```json
/* 1 createdAt:25/02/2020 à 09:01:57*/
{
	"_id" : ObjectId("5e54d475bc49fe570046e3be"),
	"ActivityId" : "610168e4-5604-40d5-b6bb-ada11ab76fee",
	"Date" : ISODate("2020-02-25T09:01:56.679+01:00"),
	"Level" : "Info",
	"StackTrace" : "StackBuilderSink._PrivateProcessMessage => PLogger...",
	"Message" : "Test",
	"Logger" : "PierroD.ExportIntegrateur",
	"ThreadID" : 1,
	"ProcessID" : 22272,
	"ProcessName" : "C:\\Program Files (x86)\\visual\\test\\test.exe",
	"UserName" : "PierroD"
},
```

## <a name="solution"></a> 4️⃣ Find aggregate date by hour

---

- Find hour = 14

```jql
db.Log.aggregate([
    { $project: {
        ActivityId: "$ActivityId",
        Date: { $hour: "$Date" },
        Level: "$Level",
        StackTrace: "$StackTrace",
        Message: "$Message",
    }},
{ $match: { Date: { "$in": [14] } } }]);

```

- Find hour = 8 or hour = 14

```jql
db.Log.aggregate([
    { $project: {
        ActivityId: "$ActivityId",
        Date: { $hour: "$Date" },
        Level: "$Level",
        StackTrace: "$StackTrace",
        Message: "$Message",
    }},
{ $match: { Date: { "$in": [8,14] } } }]);

```

- Result

```json
/* 1 createdAt:24/02/2020 à 15:03:08 because it's UTC+1 (14+1 = 15) and the date is stored like ......14:.....Z which means hour + 1*/
{
	"_id" : ObjectId("5e53d79cbc49fe33141539d2"),
	"ActivityId" : "1d57bf16-8bb8-4e7c-8758-d26ecc10848c",
	"Date" : 14,
	"Level" : "Info",
	"StackTrace" : "StackBuilderSink._PrivateProcessMessage => PLogger...",
	"Message" : "Test",
},
```
