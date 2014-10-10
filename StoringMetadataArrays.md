##Storing Social Media Data in Relational Databases (Part 2)

###Storing Metadata Arrays
Twitter data is dynamic in nature, and includes several types of metadata that are in arrays of variable length. For example, tweets can consist of multiple hashtags, urls, user mentions, and photographs. For example the tweet below contains four hashtags: #Boulder, #boulderflood, #cuboulder and #cowx

<blockquote class="twitter-tweet" lang="en"><p>Flash Flood Warning for <a href="https://twitter.com/hashtag/Boulder?src=hash">#Boulder</a> extended to 4:15AM. <a href="https://twitter.com/hashtag/boulderflood?src=hash">#boulderflood</a> <a href="https://twitter.com/hashtag/cuboulder?src=hash">#cuboulder</a> <a href="https://twitter.com/hashtag/cowx?src=hash">#cowx</a></p>&mdash; CU-Boulder Police (@CUBoulderPolice) <a href="https://twitter.com/CUBoulderPolice/statuses/378025418353573888">September 12, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

JSON readily supports arrays of data, while schemas are static in nature.  The concept of having a database field 'grow' to store dynamic array lengths of data does not exist in realtional databases.  To manage this strutural incongruity, below are three basic schema design strategies. For each example some pseudo-code, with a strong Ruby ascent, is provided. These code examples illustrate SQL queries and loading the hashtags into an array. 

#### 1) Store delimited lists in a single field:

|  id  | hashtags                  	|  
|---------------------	| ---------|
|378025418353573888 | Boulder, boulderflood, cuboulder, cowx |

One advantage of this schema design is that it results in a simple schema, enabling very simple SQL queries to retreive the data. 

```
SELECT hashtags 
FROM activities 
WHERE id = 378025418353573888;
```

Client code needs to 'split' the field contents using the (mutually agreed on) delimiter and then iterate through the results:

```
#SQL query to select a single set of hashtags for a specified tweet ID.
query = "SELECT hashtags FROM activities WHERE id = #{activity_id};"
db.connect if not db.active
result_set = db.execute(query)

hashtags = Array.new #Will load hashtags into an array.
delimiter = ',' #Need to know how hashtags are delimited. Could have self-discovering logic here.

result_set.each do |row|  
   row.each do |key, value|  #Our query specified a single field, getting back field name, value...
      hashtags_delimited << value 
   end
end

hashtags = hashtags_delimited.split(delimiter) #The joys of Ruby (and Python).
```

---------------------------------------
#### 2) Create a set of fields to hold multiple instances:

| id  | hashtag_1  | hashtag_2   | hashtag_3  | hashtag_4  | hashtag_5  | 
|----------|----------|------------|------------|------------|----------|
|378025418353573888 |  Boulder | boulderflood | cubolder | cowx        |            | 

With this method hashtags are stored in a set of fields such as hashtag_1, hashtag_2, hashtag_3, hashtag_4, and hashtag_5. For short-content sources like Twitter, limited to 140 characters, there is a good chance that there is a reasonable upper-limit on the number of items you need to support.

One disadvantage with this method is that you are likely to end up with a lot of empty fields since most tweets have just one or two hashtags. Another is that the design 'hard-codes' the number of entities you can store, so you need to decide how many to support. Yet another disadvantage is the SQL you need to write to process these multiple fields, where the client code and its query also hard-codes an explicit number of metadata items: 

```
SELECT hashtag_1, hashtag_2, hashtag_3, hashtag_4, hashtag_5 
FROM activities 
WHERE id = 378025418353573888;
```

Other than the query being completely coupled to the schema details, the client-side code is much the same although it does not need any delimiter metadata.

```
   #SQL query to select a single set of hashtags for a specified tweet ID.
   query = "SELECT hashtag_1, hashtag_2, hashtag_3, hashtag_4, hashtag_5 FROM activities WHERE id = #{activity_id};";)"
   db.connect if not db.active
   
   hashtags = Array.new #Will load hashtags into an array.

   result_set = db.execute(query)

   result_set.each do |row|  
      row.each do |k, v|
         hashtags << v #Just grab hashtag values, ignoring "hashtags" key (field name).
      end
   end
```   

---------------------------------------
####3) Create separate tables to hold multiple instances:

Activity table entry:

|    id    |                body                     |
|----------|-----------------------------------------|
|378025418353573888 | Flash Flood Warning for #Boulder extended to 4:15AM. #boulderflood #cuboulder #cowx |

Hashtag table entries:

| id |    activity_id     |  hashtag  |
|----|--------------------|-----------|
| 1  | 378025418353573888 |  Boulder     |
| 2  | 378025418353573888 |  boulderflood   |
| 3  | 378025418353573888 |  cuboulder |
| 4  | 378025418353573888 |  cowx    |

This method relies on having a separate table to store hashtags, with each row containing a single hashtag. So with our example tweet, there are four entries in this table, with the activity ID being the unique link (foreign key) between the two tables. 
```
SELECT ht.* FROM hashtags ht, activities a
WHERE ht.activity_id = a.id
AND a.id = 378025418353573888;
```
This design readily handles the dynamic '3-d' nature of JSON objects. Indeed, one advantage of this design is that there are not a predetermined number of hashtag fields for each activity and instead the hashtag table dynamically stores the array items as needed.

```
   #SQL query to select a single set of hashtags for a specified tweet ID.
   query = "SELECT ht.* FROM hashtags ht, activities a WHERE ht.activity_id = a.id AND a.id = #(activity_id};"
   db.connect if not db.active
   
   hashtags = Array.new #Will load hashtags into an array.

   result_set = db.execute(query)

   result_set.each do |row| #Will get one row for each entry in hashtags table... 
      row.each do |k, v|
         hashtags << v  
      end
   end
```   
 
###Previous:
 * [Introduction](https://github.com/twitterdev/dev-articles/blob/master/relational_db/db_intro.md)
 
###Next: 
* [Storing Dynamic and Static Metadata](https://github.com/twitterdev/dev-articles/blob/master/relational_db/db_static_data.md)
* [Some Example Schemas](https://github.com/twitterdev/dev-articles/blob/master/relational_db/db_example_schemas.md)
