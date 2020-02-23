# Examen final

[Certificado](http://university.mongodb.com/course_completion/24879e9c-7fdd-4fa8-9109-501982579300)

### 6 Items

Cualquier tema de este curso podría abordarse en el examen final.

## Question 1

**Problem:**

Connect to our **class Atlas cluster** from Compass and view the `citibike.trips` collection.

Use the schema view and any filters you feel are necessary to determine the range of values for the `usertype` field. Which of the following are values found in this collection for the field `usertype`?

Check all answers that apply:


* "Anonymous"

* "Customer" :+1:

* "Subscriber" :+1:

* null

* "Pay As You Go"


Solution:

Using the Compass schema view you can see that the majority of documents have the value "Subscriber" for this field. The rest have the value "Customer". One way to be sure that no documents have any other value is by using the $nin operator in a filter as follows.

```sh
{usertype: {$nin: ["Subscriber", "Customer"]}}
```

MIO

```sh
{$and: [{usertype: {$ne: "Custome"}}, {usertype: {$ne: "Subscribe"}} ]}
```


## Question 2

**Problem:**

Connect to our **class Atlas cluster** from Compass and view the `100YWeatherSmall.data` collection.

Using the Schema view, explore the `wind` field. The `wind` field has the value type of `document`. Which of the following best describes the schema of this embedded document?

Choose the best answer:


* Two fields -- one field with the value type "string", one with a value type of "document"

* Three fields -- all with the value type "document"

* Three fields -- two with the value type "string", one with the value type of "int32"

* Three fields -- two with the value type "document", one with the value type "string" :+1:

* Two fields -- both with the value type "document"

## Question 3

**Problem:**

Connect to our **class Atlas cluster** from Compass and view the `100YWeatherSmall.data` collection.

What is the value type of the "wind.speed.rate" field?

Choose the best answer:

* string

* document

* array

* double :+1:

* int32

## Question 4

**Problem:**

Please connect to our **class Atlas cluster** and view the `citibike` database.

How many documents in the `citibike.trips` collection have the key `tripduration` set to `null`? Ignore any documents that do not contain the `tripduration` key.

Attempts Remaining:3 Attempts left

Choose the best answer:


* 2 :+1:

* 3

* 11

* 47

* 114


Solution:

There are two documents in `citibike.trips` that have the tripduration key set to `null`. You can find this answer in the mongo shell or in `Compass`.

In the mongo shell, assuming you've connected to the M001 class Atlas cluster, you can issue the following commands to find this value.

```sh
use citibike

db.trips.find({$and: [{tripduration: null}, {tripduration: {$exists: true}}]}).count()
```

In Compass, navigate to the citibike.trips collection and then apply the following filter in either the Schema or Documents view.

```sh
{$and: [{tripduration: null}, {tripduration: {$exists: true}}]}
```

MIO

```sh
{$and: [{tripduration: {$eq: null}}, {tripduration: {$exists: true}} ]}
```

## Question 5

**Problem:**

Please connect to our **class Atlas cluster** and view the `video.movies` collection.

Which of the queries below would produce output documents that resemble the following. Check all that apply.

```sh
{ "title" : "P.S. I Love You" }
{ "title" : "Love Actually" }
{ "title" : "Shakespeare in Love" }
```

NOTE: We are not asking you to consider specifically which documents would be output from the queries below, but rather what fields the output documents would contain.

Check all answers that apply:


* db.movies.find({title: "Muppets from Space"}, {title: 1})

* db.movies.find({year: 1964}, {title: 1, _id: 0}) :+1:

* db.movies.find({}, {title: 1})

* db.movies.find({}, {title: 1, _id: 0}) :+1:

* db.movies.find({}, {title})

* db.movies.find({title: ""}, {title: 1})


Solution:

The key to answering this question is in understanding how projection works in find() queries. Specifically, we're looking for queries that include projection documents such as the following.

```sh
{title: 1, _id: 0}
```

## Question 6

**Problem:**

Please connect to our **class Atlas cluster** from the mongo shell or Compass and view the `video.movies` collection.

How many movies match the following criteria? - The cast includes either of the following actors: "Jack Nicholson", "John Huston". - The `viewerRating` is greater than 7. - The `mpaaRating` is "R".

Choose the best answer:

* 1

* 5

* 8 :+1:

* 19

* 26

Solution:

You can find this answer in the mongo shell or in Compass.

In the mongo shell, assuming you've connected to the M001 class Atlas cluster, you can issue the following commands to find this value.

```sh
use video

db.movies.find({cast: {$in: ["Jack Nicholson", "John Huston"]}, viewerRating: {$gt: 7}, mpaaRating: "R"}).count()
```

In Compass, navigate to the `video.movies` collection and then apply the following filter in either the Schema or Documents view.

```sh
{cast: {$in: ["Jack Nicholson", "John Huston"]}, viewerRating: {$gt: 7}, mpaaRating: "R"}
```

MIO

```sh
{$and: [{cast: {$in: ["Jack Nicholson", "John Huston"]}}, {viewerRating: {$gt: 7}}, {mpaaRating: "R"}]}
```

## Conclusion

Congratulations!

You've just finished all the chapters. Now you can see your course grade and download your proof of completion.

Your Final Grade:

100%
Passed

[Certificado](http://university.mongodb.com/course_completion/24879e9c-7fdd-4fa8-9109-501982579300)

