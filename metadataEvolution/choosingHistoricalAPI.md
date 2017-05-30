---
layout: help_article_nav
category: article
permalink: /articles/choosing-historical-api.html
title: Choosing a Historical API  
author: Jim Moffitt
description: Understanding the similarities and differences between the Historical PowerTrack and Full-Archive Search APIs will help you pick the appropriate tool for your historical research.
tags: historical twitter rules
path:
- name: Articles
  link: /articles/
heading: Choosing an Historical API
nav:
- name: Introduction
  link: intro
- name: Product Differences
  link: differences
- name: Selecting a Historical Product
  link: choosing
- name: Next Steps
  link: nextSteps  

---

## Choosing an Historical API 

### Introduction <a id="introduction" class="tall">&nbsp;</a>  

Both Historical PowerTrack and Full-Archive Search provide access to any publicly available Tweet, starting with [the first Tweet from March 2006](https://twitter.com/jack/status/20). Deciding which product better suits your use case can be key in getting the data you want when you need it. This article highlights the differences between the two historical products, with a goal of helping you determine which one to use for generating your next historical Tweet dataset.

Both historical products scan a Tweet archive, examine each Tweet posted during the time of interest, and generate a set of Tweets matching your query. However, Historical PowerTrack and Full-Archive Search are based on significantly different archive architectures, resulting in fundamental product differences. These differences include supported PowerTrack Operators, the number of rules/filters per request, how estimates are provided, and how the data is delivered.

We'll start off by providing an overview of Historical PowerTrack and the Full-Archive Search API, then discuss these differences in detail. We'll wrap up the discussion with general guidance for selecting a historical product.

#### Historical PowerTrack <a id="hpt" class="tall">&nbsp;</a>  

Historical PowerTrack is built to deliver Tweets at scale using a batch, Job-based design where the API is used to move a Job through multiple phases. These phases include volume estimation, Job acceptance/rejection, getting Job status, and downloading potentially many thousands of data files. Depending on the length of the request time period, Jobs can take hours or days to generate. A data file is generated for each 10-minute period that contains at least one Tweet. Therefore, a 30-day datasets will commonly consists of approximately 4,300 files regardless of the number of matched Tweets. 

The first Twitter archive was a file-based system, with each file containing a short duration of the real-time Tweet firehose. Historical PowerTrack, launched in 2012 by Gnip, was built on top of the file-based archive. As Historical PowerTrack generates your dataset, it performs file operations as it opens each file, and examines each Tweet in the file. During this process, Historical PowerTrack accesses all sections of the JSON payload that have an associated PowerTrack Operator. Historical PowerTrack supports the full set of PowerTrack Operators supported by real-time PowerTrack. 

#### Full-Archive Search <a id="search" class="tall">&nbsp;</a>  

Full-Archive Search delivers matched Tweets is a way analogous to Google Search. When you search for a particular term, Google Search does not return or display the millions of matching results all at once. Rather, it delivers a small number of results per scrollable page and allows you to click “Next” for the subsequent page of results… and so on, until all of the matching items are returned. The Full-Archvie Search API is designed to deliver smaller datasets, one at a time, with the ability to paginate for more data as needed. It is also the best solution for those situations where quick responses is needed. Thanks to being based on an index, it offers near-immediate responses to these smaller, paginated data requests. 

Full-Archive Search is designed using the classic RESTful request/response pattern, where a single PowerTrack rule is submitted and a response with matching Tweets is immediately provided. Full-Archive Search can provide a maximum of 500 Tweets per response, and a ‘next’ token is provided to paginate until all Tweets for a query are received. The more data that matches your query the longer it will take you to retrieve all of that data through the product. This is because you will need to stitch together the paginated result sets, one after the other, to create the complete set of all returned data. 

Many use cases focus on data volumes, rather than the actual Tweet messages themselves. Full-Archive Search supports a ‘counts’ endpoint that returns a time-series of the number of matched Tweets. These Tweet counts are returned in a time-series of minute-by-minute, hourly, or daily totals.

Note that Twitter also provides a 30-Day Search API. If you only need data from the last 30 days, this API may be the best match for your use case.

### Fundamental Differences <a id="differences" class="tall">&nbsp;</a>  

Here are the fundamental differences between Historical PowerTrack and Full-Archive Search:

+ **Number of rules supported per request**

    + Full-Archive Search accepts a single rule per request. 
    + A Historical PowerTrack Job can support up to 1,000 rules. 
    Note: With each product a single rule can contain up to 2,048 characters.
    
+ **How data is delivered**

     + Historical PowerTrack generates a time-series of data files, each covering a ten-minute period. For example, each hour of data is provided in six 10-minute data files (assuming each 10-minute period has at least one Tweet. If not, no file is generated). Inside each Historical PowerTrack file, the JSON Tweet payloads are written in an atomic fashion, and are not presented in an JSON array. File contents need to be parsed using newline characters as a delimiter.
     + With Full-Archive Search, Tweets in each response are arranged in a “results” array. A maximum of 500 Tweets are available per response and a ‘next’ token is provided if more Tweets are available. For example, if a 60-day request for a single PowerTrack rule matches 10,000 Tweets, at least 20 requests must be made of the Search API.
     
+ **Supported PowerTrack Operators**

    + While the majority of Operators supported by HPT are also supported by FAS, there are a set of Operators not available in FAS:
 
<table class="tg">
  <tr>
    <td class="tg-yw4l">bio:</td>
    <td class="tg-yw4l">profile_bounding_box</td>
  </tr>
  <tr>
    <td class="tg-yw4l">bio_location:</td>
    <td class="tg-yw4l">profile_point_radius</td>
  </tr>
  <tr>
    <td class="tg-yw4l">bio_name:</td>
    <td class="tg-yw4l">profile_subregion:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">contains:</td>
    <td class="tg-yw4l">retweets_of_status_id:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">is:quote</td>
    <td class="tg-yw4l">sample:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">followers_count:</td>
    <td class="tg-yw4l">source:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">friends_count:</td>
    <td class="tg-yw4l">statuses_count:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">has:lang</td>
    <td class="tg-yw4l">url_contains:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">in_reply_to_status_id:</td>
    <td class="tg-yw4l">url_description:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">listed_count:</td>
    <td class="tg-yw4l">url_title:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">time_zone:</td>
    <td class="tg-yw4l"></td>
  </tr>
</table> 
    
+ See [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/historicalOperatorsTable.md) for a side-by-side comparison of available Operators.
    + For a complete list of Operators for each product see here:
        + [Search Operator List](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators)
        + [Historical PowerTrack Operator List](http://support.gnip.com/apis/powertrack2.0/rules.html#Operators)
        
+ **Data Volume Estimates**

    + Full-Archive Search provides a 'counts' endpoint that is used to generate a minutely, hourly, or daily time-series of matching Tweets. For use cases that benefit from knowing about data *volumes*, in addtion to the actual data, the Full-Archive Search 'counts' endpoint is the tool of choice. Note that the 'counts' endpoint is a measure of *pre-compliant* matched Tweets. Pre-compliant means the Tweet totals do not take into account deleted and protected Tweets. So the 'counts' total includes every matched Tweet ever posted, but data requests will not include those unavailable deleted or private Tweets. 
    
    + The Historical PowerTrack API provides an *order of magnitude* estimate for the number of Tweets a Job will match. These estimates are based on a sampling of the time period to be covered, and should be treated a directionally accurate guide to the amount of data a historical Job will return. An Historical PowerTrack estimate will help answer whether a Job will match 100,000 or 1,000,000 Tweets. The goal is to provide reasonable expectations around the amount of data a request will return, and the Historical PowerTrack API should not be used as an estimate tool. 
    
+ **Product licensing and pricing**   

Both the Full-Archive Search and Historical PowerTrack APIs are available with 12-month subscriptions. In addition, Historical PowerTrack is available on a *'one-off'* basis. So if you need historical Tweet data for a one-time project or ad hoc research, Historical PowerTrack will be better suited to your needs. 

Also, the infrastructure and processing requirements to support Full-Archive Search are significantly higher than Historical PowerTrack. Accordingly, the licensing fees for Full-Archive Search are higher than Historical PowerTrack and the 30-Day Search API. If you only need data from the last 30 days, the 30-Day Search API is probably the best match for your use case.

    
### Selecting a Historical Product <a id="choosing" class="tall">&nbsp;</a>  

Our general recommendation is that Full-Archive Search is best suited for datasets of a few million Tweets or less. In other words, Full-Archive Search is best for collecting lower-volume datasets, while Historical PowerTrack is more appropriate for higher-volume datasets.  

This recommendation is intentionally vague as there is no real technical reason why Full-Archive Search could not be used for very large data requests. However, depending on how you plan to retrieve the data and utilize API rate limits, there actually is a threshold where Historical PowerTrack actually processes the data faster that Full-Archive Search. Historical PowerTrack typically processes a 30-day duration job in about 6 hours, regardless of the volume of matching data returns. To pull 6 million Tweets via Full-Archive Search, it would require a minimum of 12,000 search requests. If you also assume 2-second response times and an app that paginates with a single thread, it would actually take Full-Archive Search longer to retrieve this 6 million Tweet dataset.

Beyond data volume considerations and comparing completion times, there are other reasons why one historical product is best suited for your use case. Historical PowerTrack is the right product if you require any of the Operators that are not currently supported in Full-Archive Search (see above). Historical PowerTrack is also better suited for large query rulesets, as the Search API products only support a single PowerTrack rule per request. Historical PowerTrack, on the other hand, supports up to one thousand (1,000) rules. Finally, Historical PowerTrack is a great choice for use cases where receiving, in real-time, new Tweet attributes of interest (e.g. hashtags, mentions and URLs) trigger a historical request. Since Historical PowerTrack supports the same full set of Operators as real-time PowerTrack, you can always 'plug in' the corresponding real-time rules into a Historical PowerTrack Job. 

On the otherhand, Full-Archive Search is a better choice if you are building tools that depend on near-instance results and data volume estimates. Quick responses are key if you are building historical Tweet results into a dashboard or user application. Full-Archive Search has been built into many systems that enable users to create filters and instantly inspect Tweets that match. Depending on initial responses, users can continue to paginate through the results, or instead revise the filter and retry. Since Full-Archive Search provides more accurate volume estimates with its 'counts' endpoint, another common Search use case is experimenting with filters before adding them to a real-time stream. 

### Next Steps <a id="nextSteps" class="tall">&nbsp;</a>

+ [Learn more about Historical PowerTrack](http://support.gnip.com/apis/historical_api2.0/)
+ [Learn more about the Full-Archive Search API](http://support.gnip.com/apis/search_full_archive_api/)
+ [Learn more about the evolution of Tweet metadata](http://support.gnip.com/articles/tweet-timeline.html)
+ [Learn more about Historical PowerTrack and its metadata and filtering timeline](http://support.gnip.com/articles/hpt-timeline.html)
+ [Learn more about the Full-Archive Search API and its metadata and filtering timeline](http://support.gnip.com/articles/fas-timeline.html)


