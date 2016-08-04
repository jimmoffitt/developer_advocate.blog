#Migrating code to Gnip 2.0 

##Introduction

PowerTrack, Decahose, 30-Day Search, and Historical PowerTrack are now available on the Gnip 2.0 platform. These new, enhanced versions of our core API products offer new features and functionality that will give access to new enrichments, new filtering capabilities, as well as increased reliability from having multiple data centers. .

You will need to migrate all your existing Gnip APIs to the 2.0 platform by December 1, 2016 at which point we will be sunsetting all 1.0 endpoints. The purpose of this article is to dig into the details that you and your developer colleagues will need to consider to migrate to Gnip 2.0 products. 

The migration process is pretty straightforward, although there are many details to consider. There are two main areas that will require attention during the migration process: the plumbing and the filtering. This article focuses on the plumbing details, while a detailed discussion of migrating PowerTrack rules is available HERE. 


All Gnip 2.0 Products 


PowerTrack, Historical PowerTrack, 30-Day Search, and Volume Streams
Regardless of the Gnip products you use, there are migration steps that are common to all of them:

Review Migration Guides at support.gnip.com.
Update endpoint URLs to new versions.
Review new Gnip Enrichment offerings and consider how they can help with your use case.
Gnip Enrichments available in ‘original’ format. 
For filtered products (PowerTrack, Historical PowerTrack, and 30-Day Search), review the changes in rule Operators:
New Operators
Operators with new grammar.
Deprecated Operators.
Language classification.

##Individual Product Details

Beyond the general to-dos listed above, below are some more product-specific migration details to consider.

### Realtime PowerTrack Migration Guide
New URLs:
Version 1.0:
Version 2.0:
With version 2.0, the connection heartbeat interval drops from every 15 seconds to 10 seconds. This provides the opportunity to shorten your data read timeout intervals.
Rules API 2.0 
Deleting rules
Rule IDs
Updates in response payloads.
Volume Streams Migration Guide
<the above, without the filtering details>

### Historical PowerTrack <Migration Guide>
New URLs:
Version 1.0:
Version 2.0:
track_v2

New Gnip Enrichment metadata. [] Dates of new metadata
Rule Syntax and Filtering Updates:
Support for longer rules.
New Operators.
Replaced and Deprecated Operators.
Changes in tokenization and matching behavior.
New Operators for matching on new Gnip Enrichment metadata.

### 30-Day Search Migration Guide


30-Day Search 2.0 is based on the Full-Archive Search (FAS) platform. So if you have already added FAS to your package you are already familiar with the differences between 30-Day Search versions. 

New URLs:
Version 1.0:
Data: https://search.gnip.com/accounts/{ACCOUNT_NAME}/search/{LABEL}.json
Counts: https://search.gnip.com/accounts/{ACCOUNT_NAME}/search/{LABEL}/counts.json
Version 2.0:
Data: https://gnip-api.twitter.com/search/30day/accounts/{ACCOUNT_NAME}/{LABEL}.json
Counts: https://gnip-api.twitter.com/search/30day/accounts/:accountName/:label>/counts.json
New Gnip Enrichment metadata.
Rule Syntax and Filtering Updates:
Support for longer rules.
New Operators.
Replaced and Deprecated Operators.
Changes in tokenization and matching behavior.
New Operators for matching on new Gnip Enrichment metadata.

If you are not already using FAS, these videos are another resource for exploring differences between versions of 30-Day Search:
http://support.gnip.com/apis/search_full_archive_api/videos.html#CoreSearchConcepts
http://support.gnip.com/apis/search_full_archive_api/videos.html#WhatsNew
http://support.gnip.com/apis/search_full_archive_api/videos.html#NewMatchingBehavior





{We look forward to working with you as you migrate to Gnip 2.0. We trust that you’ll find the new filtering options and….. }



Be sure to check out our support documentation and contact your account manager or data-sales@twitter.com to get started with your migration today. 

