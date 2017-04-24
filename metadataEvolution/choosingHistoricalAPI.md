+ [Introduction](#introduction)
    + [Full-Archvie Search API](#search)
    + [Historical PowerTrack](#hpt)
+ [Fundamental Differences](#differences)
+ [Selecting a historical product](#choosing)
+ [Switching between products: development checklists](#switchin)

## Choosing an Historical API 

### Introduction <a id="introduction" class="tall">&nbsp;</a>  

Both Historical PowerTrack and Full-Archive Search provide access to any publicly available Tweet, starting with [the first Tweet from March 2006](https://twitter.com/jack/status/20). Deciding which product better suits your use case can be key in getting the data you want when you need it. This article highlights the differences between the two and will hopefully help you determine which product to use for producing your next historical Tweet dataset.

Architecturally, Historical PowerTrack and Full-Archive Search are significantly different. Both products scan a Tweet archive of your period of interest, examine each Tweet posted during that time, and generate a set of matching results. The product differences begin with the datastore each product serves data from. 

#### Historical PowerTrack <a id="hpt" class="tall">&nbsp;</a>  

Historical PowerTrack is built to deliver Tweets at scale using a batch, Job-based design where the API is used to move a Job through multiple phases. These phases include volume estimation, Job acceptance/rejection, getting Job status, and downloading potentially many thousands of data files. Depending on the length of the request time period, Jobs can take hours or days to generate.

The first Twitter archive was a file-based system, with each file containing a short duration of the real-time Tweet firehose. Historical PowerTrack, launched in 2012 by Gnip, was built on top of the file-based archive. As HPT generates your dataset, it performs file operations as it opens each file, and examines each Tweet in the file. During this process, HPT access all sections of the JSON payload that have an associated PowerTrack Operator. This set of fifty-some Operators includes the ```contains:``` Operator, meaning HPT even performs substring matching of historical Tweets.

[More: create Job, estimate provided, accept job, 10-minute data files are compiled, user downloads.]


#### Full-Archive Search <a id="search" class="tall">&nbsp;</a>  

Full-Archive Search is most analogous to Google Search. When you search for a particular term, Google Search doesn’t return or display the millions of matching results all at once. Rather, it delivers a small number of results per scrollable page and allows you to click “Next” for the subsequent page of results… and so on, through all of the matching items returned. Like Google, the FAS tool is best-suited for delivering smaller datasets at a time with the ability to paginate for more data as needed. It is also the best solution for those situations where expediency in returns is needed. Thanks to being based on an index, it offers near-immediate responses to these smaller data queries. Using Full-Archive Search, the more data that matches your query the longer it will take you to retrieve all of that data through the product. This is because you will need to stitch together the paginated groups of results, one after the other, to create the complete file of all returned data. 



So Full-Archive Search is designed using the classic request/response pattern, where a single PowerTrack rule is submitted and a response with matching Tweets is immediately provided. Full-Archive Search can provide a maximum of 500 Tweets per response, and a ‘next’ token is provided to paginate until all Tweets for a query are received. 

Full-Archive Search also supports ‘count’ requests, where only the number of matching Tweets is provided, These counts are returned in a time-series of minute-by-minute, hourly, or daily totals.



Next, we'll dig into these fundamental differences between Historical PowerTrack and Full-Archive Search:
+ Number of rules supported per request.
+ Available PowerTrack Operators. 
+ API methods for delivering data. 
+ Data volume estimation methods.

### Fundamental Differences <a id="differences" class="tall">&nbsp;</a>  

Here are the fundamental differences between Historical PowerTrack (HPT) and Full-Archive Search (FAS):

+ Number of rules supported per request.
    + Full-Archive Search accepts a single rule per request. Each rule can be up to 2,048 characters. 
    + A Historical PowerTrack Job can support up to 1,000 rules. 
+ API Data Response.
     + HPT generates a time-series of data files, each covering a ten-minute period. For example, each hour of data is provided in six 10-minute data files. Inside each HPT file, the JSON Tweet payloads are written in an atomic fashion, and are not presented in an JSON array. File contents need to be parsed using newline characters as a delimiter.
     + With FAS, Tweets in each response are arranged in a “results” array. A maximum of 500 Tweets are available per response and a ‘next’ token is provided if more Tweets are available. For example, if a 60-day request for a single PowerTrack rule matches 10,000 Tweets, at least 20 requests must be made of the Search API.
+ Supported PowerTrack Operators.
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
    <td class="tg-yw4l">emoji</td>
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
    <td class="tg-yw4l">time_zone:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">in_reply_to_status_id:</td>
    <td class="tg-yw4l">url_contains:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">is:quote</td>
    <td class="tg-yw4l">url_description:</td>
  </tr>
  <tr>
    <td class="tg-yw4l">listed_count:</td>
    <td class="tg-yw4l">url_title:</td>
  </tr>
</table>

+ See [HERE](https://github.com/jimmoffitt/developer_advocate.blog/blob/master/metadataEvolution/historicalOperatorsTable.md) for a side-by-side comparison of available Operators.
+ For a complete list of Operators for each product see here:
    + [Search Operator List](http://support.gnip.com/apis/search_full_archive_api/rules.html#Operators)
    + [Historical PowerTrack Operator List](http://support.gnip.com/apis/powertrack2.0/rules.html#Operators)
        


### Selecting a historical product <a id="choosing" class="tall">&nbsp;</a>  



Search:
+ Dashboards, quick estimates
+ "first 500" tweets that a rule, single query matches

HPT: 
+ Batch of rules to apply to a common time period
+ Need filtering Operators not available in Search APIs
+ Many millions of Tweets



A general assumption to be made here is that Full-Archive Search is better-suited for lower-volume jobs, while Historical PowerTrack is more appropriate for higher-volume jobs and use cases. We’ve intentionally left those descriptions relatively vague, though, as there is no real technical reason why Full-Archive Search could not be used for large data requests. 

Historical PowerTrack is a good first choice for retrieving Tweets at scale, where a result set is more than a few million Tweets.  

Depending on how you plan to retrieve the data and utilize rate limits, there actually is a threshold where Historical PowerTrack actually processes the data faster that Full-Archive Search. Historical PowerTrack typically processes a 30-day duration job in about 3 hours [CONFIRM, CURRENT PERFORMANCE?], regardless of the volume of matching data returns. To pull 2.7M Tweets via Full-Archive Search, it would require a minimum of 5,400 search requests. If you also assume 2 second response times and the need to serially paginate through the data, it would actually take Full-Archive Search longer to pull all of the Tweets. {-There are optimizations you can make to retrieve the data faster via Full-Archive Search through parallel requests, but we can discuss those separately}.

Historical PowerTrack is also the right product if you require certain operators that aren’t currently supported in Full-Archive Search (see above). Historical PowerTrack is also a better solution for large, complex rules sets and/or when Operators from its expanded list of Operators is needed.

Historical PowerTrack is also better suited for large query rulesets, as the Search API products only support a single PowerTrack rule per request. Historical PowerTrack, on the other hand, supports up to one thousand (1,000) rules. 







### Switching between products: development checklists <a id="switching" class="tall">&nbsp;</a>  

Many of our customers use both HPT and FAS. If you are new to both, or one of these, below are checklists that describe steps to migrate a historical request from one product to that other. 

#### Steps for converting a HPT Job to Full-Archive Search:

+ Integrate the Full-Archive Search (FAS) API.
    + FAS product documentation is available [HERE](http://support.gnip.com/apis/search_full_archive_api/).
    + FAS API documentation is available [HERE](http://support.gnip.com/apis/search_full_archive_api/api_reference.html).
    + If already using 30-Day Search, the same code used for 30-Day Search can be used for Full-Archive Search.
    + Example client code is available [HERE](http://support.gnip.com/code/search_api.html).
+ Review HPT rules and confirm all Operators are supported by Search API.
+ Multiple rules associated with a HPT Job need to be split up into individual Search requests. Each rule will be individually requested from the FAS API.
+ When receiving API responses:
    + Tweets are returned in a “results” array, starting with most recent Tweets.
    + A maximum of 500 Tweets are provided per response.
    + A ‘next’ token is provided if more requests are needed to complete request. The ‘next’ token should be parsed from the response and added to the next API request.

#### Steps for converting FAS requests to a HPT Job:

+ Integrate the Historical PowerTrack (HPT) API.
    + HPT product documentation is available [HERE](http://support.gnip.com/apis/historical_api2.0/).
    + HPT API documentation is available [HERE](http://support.gnip.com/apis/historical_api2.0/api_reference.html).
    + Example client code is available [HERE](http://support.gnip.com/code/historical_powertrack.html).
+ All FAS Operators are supported with HPT, so all rules should migrate over without revisions.
    + [Note any changes in matching behavior...]    
+ If you have a use-pattern where you query a common time period with multiple queries, these queries should be combined into a single HPT Job. 
+ The HPT API is used to advance a Job through its life cycle. 
    + When the Job is completed, a *Data URL* is generated taht provides an array of files to download. 
    + A data file is created for each 10-minute period with a matching Tweet. 
    + Tweets are written to these files in an atomic fashion, and are not placed in a JSON array. Tweets are delimited with new line characaters. 
