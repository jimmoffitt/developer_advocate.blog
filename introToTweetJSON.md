# Introduction to Tweet JSON

When ingesting Tweet data the main object is the Tweet Object, which is a parent object to many child objects. For example, all Tweets include a User object that describe who authored the Tweet. If you are working with an oject is a Retweet or Quoted Tweet, then that object will contain two Tweet objects, complete with two User objects. If the Tweet is geo-tagged, there will be location and place objects included. Every Tweet includes a entities object that encapsulates arrays of hashtags, user mentions, URLs, cashtags, and native media. If the Tweet has at lease one native photo, then there is an extended_entities object with metadata for up to four photos.

These objects are encoded using JavaScript Object Notation (JSON). JSON describes objects as a hierachy of objects and attributes.
