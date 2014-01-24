**JSON-to-CSV Conversion notes**

*"Can you deliver the data in CSV?  Sure, sort of. Are you sure?"*

We get asked about converting JSON data to CSV very frequently.  This is a very common request for one-time consumers of social data.  A typical scenario is someone conducting research and exploring signals from their domain in social media data.  Hydrologists, Social Scientists, [tweeting-in-the-rain example]


**Some Background**

Comma-Separated-Values (CSV) formating is fundamentally static, a two-dimensional grid of data and information.  All spreadsheet software readily imports CSV-formatted.  CSV data is also easy to import into database as tables. There is essentially a one-to-one relationship between CSV files and data tables.

JSON formatting is dynamic in nature because it readily supports hashes and arrays of variable length. In this sense JSON can natively represent an additional dimension.  There is essentially a one-to-many relationship between JSON files and (multiple) data tables.

**Deciding what JSON attributes to convert**

One fundamental challenge of converting social media data, regardless of formats, is the sheer size of the datasets. A single activity may consist of over 150 attributes. Even a dataset with 1,000,000 social activities, a relatively small dataset, then results in 150,000,000 attributes names and values.  



**Converting JSON arrays to a single CSV field**


**Example Use-case**

The process of converting tweets from JSON to CSV was much more complicated than anticipated. To help illustrate the path taken for this project, we'll tell a short data story. We'll survey a 24-hour period looking for Twitter signals around New Years Eve 2014 in three different parts of the world.  

Exploring News Year 



**********************************************
**Example Code**


Fundamental details




*Nominating JSON arrays for 'special' flattening*

config@arrays_to_collapse = 'hashtags,user_mentions,twitter_entities.urls,gnip.urls,matching_rules,topics'



*Supporting 'special' header mappings*

Default behavior is to use the dot-notation key by default, but some keys get silly-long:

    #twitter_entities.hashtags.0.text               --> hashtags
    #twitter_entities.urls.0.url                    --> twitter_urls
    #twitter_entities.urls.0.expanded_url           --> twitter_expanded_urls
    #twitter_entities.urls.0.display_url            --> twitter_display_urls
    #twitter_entities.user_mentions.0.screen_name   --> user_mention_screen_names
    #twitter_entities.user_mentions.0.name          --> user_mention_names
    #twitter_entities.user_mentions.0.id            --> user_mention_ids
    #gnip.matching_rules.0.value                    --> rule_values
    #gnip.matching_rules.0.tag                      --> tag_values
    
The above represent the current defaults that are automatically generated. These can be updated in the (YAML) config file.

These are specified at config@header_mappings.
