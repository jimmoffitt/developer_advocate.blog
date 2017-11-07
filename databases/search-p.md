---
layout: documentation_new
category: documentation
title: Premium search API
description: API reference for the Search Tweets: 30-Day API
heading: Premium search API
nav:

- name: Introduction
  link: Introduction
- name: Methods
  link: Methods
- name: Authentication
  link: Authentication
- name: Request/response behavior
  link: RequestResponseBehavior
- name: Pagination
  link: Pagination 
- name: Data endpoint 
  link: DataEndpoint
- name: Data request parameters
  link: DataParameters
- name: Data request examples 
  link: DataRequestExamples
- name: Counts endpoint
  link: CountsEndpoint
- name: Counts request parameters 
  link: CountsParameters  
- name: Counts request Eeamples 
  link: CountsRequestExamples  
- name: HTTP response codes
  link: HTTPCodes
---

## Introduction<a id="Introduction" class="extra-tall">&nbsp;</a>

Below you will find important details needed when integrating with the Search Tweets: 30-Day API:

+ Methods for requesting Tweet data and counts
+ Pagination
+ API request parameters and example requests
+ API response JSON payloads and example responses
+ HTTP response codes

The Search Tweets: 30-Day API provides low-latency, full-fidelity, query-based access to the previous 30 days of Tweets with minute granularity. Tweet data is served in reverse chronological order, starting with the most recent Tweet that matches your query. Tweets are available from the search API approximately 30 seconds after being published.


## Methods<a id="Methods" class="extra-tall">&nbsp;</a>

The base URI for the premium search API is https:/api.twitter.com/1.1/tweets/search/.

The Search Tweets: 30-Day API provides both *data* and *counts* endpoints. Note that the counts endpoint is not available in *Sandbox* dev environments.

<table class="table table-hover">
    <tr>
        <th>
            Method</th>
        <th>
            Description</th>
    </tr>
    <tr>
        <td>
            <a href="#SearchRequests">POST /search/30day/:label</a></td>
        <td>
            Retrieve Tweets matching the specified query.</td>
    </tr>
    <tr>
        <td>
            <a href="#CountRequests">POST /search/30day/:label/counts</a></td>
        <td>
            Retrieve the number of Tweets matching the specified query.</td>
    </tr>
</table>

Where:
 
 - ```:label``` is the (case-sensitive) label associated with your Search endpoint, as displayed at https://developer.twitter.com/en/account/environments.

For example, if your dev environment has a label of 'prod' (short for production), the search URLs would be:

+ Data endpoint providing Tweets: ```https:/api.twitter.com/1.1/tweets/search/30day/prod.json```
+ Counts endpoint providing *counts* of Tweets: ```https:/api.twitter.com/1.1/tweets/search/30day/prod/counts.json```
 
Your complete search API endpoint is displayed at https://developer.twitter.com/en/account/environments.

## Authentication<a id="Authentication" class="tall">&nbsp;</a>

You have two *application-only* OAuth options when authenticating with the Search Tweets: 30-Day API: 

+ Authentication using consumer token and secret.
+ Authentication with a single Bearer token. 

If you are new to OAuth, be sure to check out [our documentation](https://developer.twitter.com/en/docs/basics/authentication/overview/oauth).

### OAuth 2 with Bearer token

If you are completely new to Twitter search APIs and/or OAuth, Bearer authentication is a good place to start. Authentication is performed by passing a Bearer token as a HTTP request header. While this application-only authentication requires you to first generate a token, after that it is easy to start making search requests with tools like curl and HTTPie. When using these types of HTTP tools you can exercise the API with a single terminal command.

Bearer tokens are based on Twitter app consumer tokens. For a curl-based recipe for generating Bearer tokens, see [HERE](https://developer.twitter.com/en/docs/basics/authentication/overview/application-only). Generating bearer tokens is equally straightforward using the language of your choice. [HERE](http://www.rubyexample.com/code/twitter%20application%20bearer%20token/) is an example in Ruby.

Request examples below use Bearer tokens.

### OAuth consumer key and secret

If you have already integrated with the standard search API, with application-only consumer key and secret, your code base should be ready to authenticate with Search Tweets: 30-Day. That code base is likely using a language-specific OAuth package that abstracts away all the underlying ‘handshake’ details. With these packages, the authentication path typically means configuring your keys and tokens, creating some sort of HTTP object, then making requests with that object. 

If you are building your first app using this form of OAuth, we recommend that you find a OAuth package for your integration language of choice and start there. For example, [this Node.js simple script](https://github.com/jimmoffitt/twitter_search/blob/master/search.js) for the Search Tweets: 30-Day API uses the Node ‘request’ package and makes a POST request. For this example, Twitter app keys and tokens are stored as environment variables. 

## Request/response behavior<a id="RequestResponseBehavior" class="tall">&nbsp;</a>

Using the ```fromDate``` and ```toDate``` parameters, you can request any time period from the last 31 days (even though referred to as the '30-Day' API, it makes 31 days avaialble to enable users to make complete month requests). Each Tweet *data* request contains a 'maxResults' parameter (range 10-500, with a default of 100. Sandbox environments have a maximum of 100) that specifies the maximum number of Tweets to return in the response. When the amount of data exceeds the 'maxResults' setting (and it usually will), the response will include a 'next' token and pagination will be required to receive all the data associated with your query (see the <a href="#Pagination">HERE</a> section for more information).

For example, say your query matches 6,000 Tweets over the past 30 days (if you do not include date parameters in your request, the API will default to the full 30-day period). The API will respond with the first 'page' of results with either the first 'maxResults' of Tweets or all Tweets from the first 30 days if there are less than that for that time perod. That response will contain a 'next' token and you'll make another call with that next token added to the request. To retrieve all of the 6,000 Tweets, approximately 12 requests will be neccessary. 

## Pagination<a id="Pagination" class="tall">&nbsp;</a>

When making both data and count requests it is likely that there are more data than can be returned in a single response. When that is the case the response will include a 'next' token. The 'next' token is provided as a root-level JSON attribute. Whenever a 'next' token is provided, there is additional data to retrieve so you will need to keep making API requests.

<b>Note:</b> The 'next' token behavior differs slightly for data and counts requests, and both are described below with example responses provided in the API Reference section.

### Data pagination

Requests for data will likely generate more data than can be returned in a single response. Each data request includes a parameter that sets the maximum number of Tweets to return per request. The 'maxResults' parameter defaults to 100, and can be set to a range of 10-500 (or a maximum of 100 with Sandbox environments). If your query matches more Tweets than the 'maxResults' parameter used in the request, the response will include a 'next' token (as a root-level JSON attribute). This 'next' token can be used in a subsequent request to retrieve the next portion of the matching Tweets for that query (i.e. the next 'page'). Next tokens will continue to be provided until you have reached the last “page” of results for that query, when no 'next' token is provided.

To request the next 'page' of data, you must make the exact same query as the original, including ```query```, ```toDate```, and ```fromDate```, if used, and also include a 'next' request parameter set to the value from the previous response. This can be utilized with either a GET or POST request. However, the 'next' parameter must be URL encoded in the case of a GET request. You can continue to pass in the 'next' element from your previous query until you have received all Tweets from the time period covered by your query. When you receive a response that does not include a 'next' element, it means that you have reached the last page and no additional data are available for the specified query and time range.

### Counts pagination

The 'counts' endpoint provides Tweet volumes associated with a query on either a daily, hourly, or per-minute basis. The 'counts' API endpoint will return a timestamped array of counts for a maximum of 31-day payload of counts. 

For higher volume queries, there is the potential that the generation of counts will take long enough to potentially trigger a response timeout. When this occurs you will receive less than 31 days of counts, but will be provided a 'next' token in order to continue making requests for the entire payload of counts. 

As with the data 'next' tokens, you must make the exact same query as the original and also include a 'next' request parameter set to the value from the previous response.

### Additional notes
+ When using a ```fromDate``` or ```toDate``` in a search request, you will only get results from within your time range. When you reach the last group of results within your time range, you will not receive a 'next' token.
+ The 'next' element can be used with any ```maxResults``` value between 10-500 (with a default value of 100. Sandbox dev environments have a maximum of 100). The ```maxResults``` parameter determines how many Tweets are returned in each response, but does not prevent you from eventually getting all results.
+ The 'next' token does not expire. Multiple requests using a the same 'next' query will receive the same results, regardless of when the request is made. Be sure to always update the 'next' token as you paginate through the results. 
+ When paging through results using the 'next' parameter, you may encounter duplicates at the edges of the query. Your application should be tolerant of these.

## Data endpoint <a class='tall' id='DataEndpoint'>&nbsp;</a>

### POST /search/30day

#### Endpoint pattern: 
/tweets/search/30day/:label.json

This endpoint returns data for the specified query and time period. If a time period is not specified the time parameters will default to the last 30 days. 
Note: This functionality can also be accomplished using a GET request, instead of a POST, by encoding the parameters described below into the URL.

### Data request parameters<a class='tall' id='DataParameters'>&nbsp;</a>

<table class='table'>
      <tr>
        <th>
            Parameters</th>
        <th>
            Description</th>
        <th>
            Required</th> 
        <th>
            Sample Value</th>       
      </tr>
      <tr>
        <th>query</th>
        <td>
            The equivalent of one PowerTrack rule, with up to 1,024 characters (256 with Sandbox dev environments).<br />
            <br />
            This parameter should include ALL portions of the PowerTrack rule, including all operators, and portions of the rule should not be separated into other parameters of the query.<br />
            <br />
            <strong>Items to Note:</strong>
            <ul>
              <li>Not all PowerTrack operators are supported. Supported Operators are listed <a href='/en/docs/tweets/search/overview/premium#AvailableOperators'>HERE</a>.</li> 
            </ul>
        </td>
        <td>Yes</td>
        <td>(snow OR cold OR blizzard) weather</td>
    </tr>
    <tr>
        <th>tag</th>
        <td>Tags can be used to segregate rules and their matching data into different logical groups. If a rule tag is provided, the rule tag is included in the 'matching_rules' attribute. <br /> 
          <br />
          It is recommended to assign rule-specific UUIDs to rule tags and maintain desired mappings on the client side. <br />
          <br />
        </td>
        <td>No</td>
        <td>8HYG54ZGTU</td>
    </tr>
    <tr>
        <th>fromDate</th>
        <td>The oldest UTC timestamp (from most recent 30 days) from which the Tweets will be provided. Timestamp is in minute granularity and is inclusive (i.e. 12:00 includes the 00 minute).<br />
        <br />
        <i>Specified:</i> Using only the fromDate with no toDate parameter will deliver results for the query going back in time from now( ) until the fromDate.<br />
        <br />
        <i>Not Specified:</i> If a fromDate is not specified, the API will deliver all of the results for 30 days prior to now( ) or the toDate (if specified).<br /> 
        <br />
        If neither the fromDate or toDate parameter is used, the API will deliver all results for the most recent 30 days, starting at the time of the request, going backwards.<br />
        </td>
        <td>No</td>
        <td>201711220000</td>
    </tr>
    <tr>
        <th>toDate</th>
        <td>The latest, most recent UTC timestamp to which the activities will be provided. Timestamp is in minute granularity and is not inclusive (i.e. 11:59 does not include the 59th minute of the hour).<br /> 
        <br />
        <i>Specified:</i> Using only the toDate with no fromDate parameter will deliver the most recent 30 days of data prior to the toDate.<br />
        <br />
        <i>Not Specified:</i> If a toDate is not specified, the API will deliver all of the results from now( ) for the query going back in time to the fromDate.<br />
        <br />
        If neither the fromDate or toDate parameter is used, the API will deliver all results for the entire 30-day index, starting at the time of the request, going backwards.</td>
        <td>No</td>
        <td>201208220000</td>
    </tr>
    <tr>
        <th>maxResults</th>
        <td>The maximum number of search results to be returned by a request. A number between 10 and the system limit (currently 500, 100 for Sandbox environments). By default, a request response will return 100 results.</td>
        <td>No</td>
        <td>500</td>
    </tr>
    <tr>
        <th>next</th>
        <td>This parameter is used to get the next 'page' of results as described <a href="#Pagination">HERE</a>. The value used with the parameter is pulled directly from the response provided by the API, and should not be modified.</td>
        <td>No</td>
        <td>NTcxODIyMDMyODMwMjU1MTA0</td>
    </tr>
</table>
<br />

#### Additional details<a class='tall' id='AdditionalDetails'>&nbsp;</a>

<table class='table'>
    <tr>
        <td><strong>Available Timeframe</strong></td>
        <td>Last 31 days</td>
    </tr>

    <tr>
        <td><strong>Query Format</strong></td>
        <td>The equivalent of one PowerTrack rule, with up to 1,024 characters (256 with Sandbox dev environment). <br />
            <br />
            <strong>Items to Note:</strong>
            <ul>
              <li>Not all PowerTrack operators are supported. Supported Operators are listed <a href='/en/docs/tweets/search/overview/premium#AvailableOperators'>HERE</a>.</li> 
            </ul>
     </td>
    </tr>
    <tr>
        <td><strong>Rate Limit</strong></td>
        <td>Request rate limits at both minute and second granularity. The per minute rate limit is 60 requests per minute (30 with Sandox environment). Requests are also limited to 10 per second. Requests are aggregaged across both the data and counts endpoints. Monthly request limits are also applied. Sandbox environments are limited to 250 requests per month, and paid access can range between 500 and 100,000 requests.</td>
    </tr>
    <tr>
        <td><strong>Compliance</strong></td>
        <td>All data delivered via the search APIs is compliant at the time of delivery.</td>
    </tr>
    <tr>
        <td><strong>Realtime Availability</strong></td>
        <td>Data is available in the index within 30 seconds of generation on the Twitter Platform</td>
    </tr>
</table>

***

### Example data requests and responses<a class='tall' id='DataRequestExamples'>&nbsp;</a>

#### Example POST request

- Request parameters in a POST request are sent via a JSON-formatted body, as shown below.
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter.
- Do not split portions of the rule out as separate parameters in the query URL.

Here is an example POST (using cURL) command for making an initial data request:

<pre>
curl -X POST "https:/api.twitter.com/1.1/tweets/search/30day/:label.json" -d '{"query":"TwitterDev \"search api\"","maxResults":"500","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;"}' -H "Authorization: Bearer TOKEN"

</pre>

Note that the rule used contains an exact phrase with quotes, so those quotes must be escaped (with a '\' character) in order for the JSON to be valid.

If the API data response includes a 'next' token, below is a subsequent request that consists of the original request, with the 'next' parameter set to the provided token:

<pre>
curl -X POST "https:/api.twitter.com/1.1/tweets/search/30day/:label.json" -d '{"query":"TwitterDev \"search api\"","maxResults":"500","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;",
"next":"NTcxODIyMDMyODMwMjU1MTA0"}' -H "Authorization: Bearer TOKEN"
</pre>

#### Example GET request<a class='tall' id='SearchGetRequest'>&nbsp;</a>

- Request parameters in a GET request are encoded into the URL, using standard URL encoding.
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter.
- Do not split portions of the rule out as separate parameters in the query URL.
- These examples use a dev environment label of 'prod' and 'TOKEN' represents a Bearer token.

Here is an example GET (using cURL) command for making an initial data request:

<pre>
curl  "https:/api.twitter.com/1.1/tweets/search/30day/:label.json?query=TwitterDev%20%5C%22search%20api%5C%22&amp;maxResults=500&amp;fromDate=&lt;yyyymmddhhmm&gt;&amp;toDate=&lt;yyyymmddhhmm&gt;" -H "Authorization: Bearer TOKEN"
</pre>

#### Example data responses<a class='tall' id='SearchGetResponse'>&nbsp;</a>

Below is an example response to a data query. This example assumes that there were more than ‘maxResults’ Tweets available so a 'next' token is provided for subsequent requests. If 'maxResults' or less Tweets are associated with your query, no 'next' token would be included in the response. <br>
The value of the 'next' element will change with each query and should be treated as an opaque string. The 'next' element will look like the following in the response body: <br>

<pre>
{
    "results":
      [
           {--Tweet 1--},
           {--Tweet 2--},
           ...
           {--Tweet 500--}
      ],
    "next":"NTcxODIyMDMyODMwMjU1MTA0",  
    "requestParameters":
      {
        "maxResults":500,
        "fromDate":"201711010000",
        "toDate":"201711030000"
      }
 }
</pre>

The response to a subsequent request might look like the following (note the new Tweets and different 'next' value): <br />

<pre>
{
      "results":
      [
           {--Tweet 501--},
           {--Tweet 502--},
           ...
           {--Tweet 1000--}
      ],
      "next":"R2hCDbpBFR6eLXGwiRF1cQ",
      "requestParameters":
      {
        "maxResults":500,
        "fromDate":"201101010000",
        "toDate":"201201010000"
      }
 }
</pre>

You can continue to pass in the 'next' element from your previous query until you have received all Tweets from the time period covered by your query. When you receive a response that does not include a 'next' element, it means that you have reached the last page and no additional data is available in your time range.

***

## Counts endpoint <a class='tall' id='CountsEndpoint'>&nbsp;</a>

### /search/:label/counts<a class='tall' id='CountRequests'>&nbsp;</a>

#### Endpoint pattern: 
/tweets/search/30day/:label/counts.json

This endpoint returns counts (data volumes) data for the specified query. If a time period is not specified the time parameters will default to the last 30 days. Data volumes are returned as a timestamped array on either daily, hourly (default), or by the minute.  

**Note:** This functionality can also be accomplished using a GET request, instead of a POST, by encoding the parameters described below into the URL.

### Counts request parameters<a class='tall' id='CountsParameters'>&nbsp;</a>

<table class='table'>
      <tr>
        <th>
            Parameters</th>
        <th>
            Description</th>
        <th>
            Required</th> 
        <th>
            Sample Value</th>       
      </tr>
      <tr>  
        <th>query</th>
        <td>
            The equivalent of one PowerTrack rule, with up to 1,024 characters (256 with Sandbox dev environments).<br />
            <br />
            This parameter should include ALL portions of the PowerTrack rule, including all operators, and portions of the rule should not be separated into other parameters of the query.<br />
            <br />
            <strong>Items to Note:</strong>
            <ul>
              <li>Not all PowerTrack operators are supported. Supported Operators are listed <a href='/en/docs/tweets/search/overview/enterprise#AvailableOperators'>HERE</a>.</li> 
            </ul>
        </td>
        <td>Yes</td>
        <td>(snow OR cold OR blizzard) weather</td>
    </tr>
    <tr>
        <th>fromDate</th>
        <td>The oldest UTC timestamp from which the Tweets will be provided. Timestamp is in minute granularity and is inclusive (i.e. 12:00 includes the 00 minute).<br />
          <br />
          <i>Specified:</i> Using only the fromDate with no toDate parameter, the API will deliver counts (data volumes) data for the query going back in time from now until the fromDate. If the fromDate is older than 31 days from now( ), you will receive a next token to page through your request.<br /> 
          <br />
          <i>Not Specified:</i> If a fromDate is not specified, the API will deliver counts (data volumes) for 30 days prior to now( ) or the toDate (if specified).<br />
          <br />
          If neither the fromDate or toDate parameter is used, the API will deliver counts (data volumes) for the most recent 30 days, starting at the time of the request, going backwards.<br />
          <br />
        </td>
        <td>No</td>
        <td>201207220000</td>
    </tr>
    <tr>
        <th>toDate</th>
        <td>The latest, most recent UTC timestamp to which the activities will be provided. Timestamp is in minute granularity and is not inclusive (i.e. 11:59 does not include the 59th minute of the hour).<br /> 
        <br />
        <i>Specified:</i> Using only the toDate with no fromDate parameter will deliver the most recent counts (data volumes) for 30 days prior to the toDate.<br />
        <br />
        <i>Not Specified:</i> If a toDate is not specified, the API will deliver counts (data volumes) for the query going back in time to the fromDate. If the fromDate is more than 31 days from now( ), you will receive a next token to page through your request.<br />
        <br />
        If neither the fromDate or toDate parameter is used, the API will deliver counts (data volumes) for the most recent 30 days, starting at the time of the request, going backwards.</td>
        <td>No</td>
        <td>201208220000</td>
    </tr>
    <tr>
        <th>bucket</th>
        <td>The unit of time for which count data will be provided. Count data can be returned for every day, hour or minute in the requested timeframe. By default, hourly counts will be provided. Options: "day", "hour", "minute"
        </td>
        <td>No</td>
        <td>minute</td>
    </tr>
    <tr>
        <th>next</th>
        <td>This parameter is used to get the next "page" of results as described <a href="#Pagination">HERE</a>. The value used with the parameter is pulled directly from the response provided by the API, and should not be modified.</td>
        <td>No</td>
        <td>NTcxODIyMDMyODMwMjU1MTA0</td>
    </tr>
</table>

#### Additional details

<table class='table'>
    <tr>
        <td><strong>Available Timeframe</strong></td>
        <td>Last 31 days</td>
    </tr>
    <tr>
        <td><strong>Query Format</strong></td>
        <td>The equivalent of one PowerTrack rule, with up to 1,024 characters (256 with Sandbox dev environments). <br />
            <br />
            <strong>Items to Note:</strong>
            <ul>
              <li>Not all PowerTrack operators are supported. Supported Operators are listed <a href='/en/docs/tweets/search/overview/enterprise#AvailableOperators'>HERE</a>.</li> 
            </ul>
        </td>
    </tr>
    <tr>
        <td><strong>Rate Limit</strong></td>
         <td>Request rate limits at both minute and second granularity. The per minute rate limit is 60 requests per minute (30 with Sandox environment). Requests are also limited to 10 per second. Requests are aggregaged across both the data and counts endpoints. Monthly request limits are also applied. Sandbox environments are limited to 250 requests per month, and paid access can range between 500 and 100,000 requests.</td>  
    </tr>
    <tr>
        <td><strong>Count Precision</strong></td>
        <td>The counts delivered through this endpoint reflect the number of Tweets that occurred and do not reflect any later compliance events (deletions, scrub geos). Some Tweets counted may not be available via data endpoint due to user compliance actions.</td>
    </tr>
</table>

### Example counts requests and responses<a class='tall' id='CountsRequestExamples'>&nbsp;</a>

#### Example POST request

- Request parameters in a POST request are sent via a JSON-formatted body, as shown below.
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter.
- Do not split portions of the rule out as separate parameters in the query URL.

Here is an example POST (using cURL) command for making an initial counts request:

<pre>
curl -X POST "https:/api.twitter.com/1.1/tweets/search/30day/:label/counts.json" -d '{"query":"TwitterDev \"search api\"","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;","bucket":"day"}' -H "Authorization: Bearer TOKEN"
</pre>

If the API counts response includes a 'next' token due to very high data count volumes, below is a subsequent request that consists of the original request, with the 'next' parameter set to the provided token:

<pre>
curl -X POST "https:/api.twitter.com/1.1/tweets/search/30day/:label/counts.json" -d '{"query":"TwitterDev \"search api\"","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;","bucket":"day",
"next":"YUcxO87yMDMyODMwMjU1MTA0"}' -H "Authorization: Bearer TOKEN"
</pre>

#### Example GET request

- Request parameters in a GET request are encoded into the URL, using standard URL encoding.
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter.
- Do not split portions of the rule out as separate parameters in the query URL.

Here is an example GET (using cURL) command for making an initial counts request:

<pre>
curl -u&lt;username&gt; "hhttps:/api.twitter.com/1.1/tweets/search/30day/:label/counts.json?query=TwitterDev%20%5C%22search%20api%5C%22&amp;bucket=day&amp;fromDate=&lt;yyyymmddhhmm&gt;&amp;toDate=&lt;yyyymmddhhmm&gt;" -H "Authorization: Bearer TOKEN"
</pre>

#### Example counts responses

Below is an example response to a counts (data volume) query. 

<pre>
{
  "results": [
    { "timePeriod": "201701010000", "count": 32 },
    { "timePeriod": "201701020000", "count": 45 },
    { "timePeriod": "201701030000", "count": 57 },
    { "timePeriod": "201701040000", "count": 123 },
    { "timePeriod": "201701050000", "count": 134 },
    { "timePeriod": "201701060000", "count": 120 },
    { "timePeriod": "201701070000", "count": 43 },
    { "timePeriod": "201701080000", "count": 65 },
    { "timePeriod": "201701090000", "count": 85 },
    { "timePeriod": "201701100000", "count": 32 },
    { "timePeriod": "201701110000", "count": 23 },
    { "timePeriod": "201701120000", "count": 85 },
    { "timePeriod": "201701130000", "count": 32 },
    { "timePeriod": "201701140000", "count": 95 },
    { "timePeriod": "201701150000", "count": 109 },
    { "timePeriod": "201701160000", "count": 34 },
    { "timePeriod": "201701170000", "count": 74 },
    { "timePeriod": "201701180000", "count": 24 },
    { "timePeriod": "201701190000", "count": 90 },
    { "timePeriod": "201701200000", "count": 85 },
    { "timePeriod": "201701210000", "count": 93 },
    { "timePeriod": "201701220000", "count": 48 },
    { "timePeriod": "201701230000", "count": 37 },
    { "timePeriod": "201701240000", "count": 54 },
    { "timePeriod": "201701250000", "count": 52 },
    { "timePeriod": "201701260000", "count": 84 },
    { "timePeriod": "201701270000", "count": 120 },
    { "timePeriod": "201701280000", "count": 34 },
    { "timePeriod": "201701290000", "count": 83 },
    { "timePeriod": "201701300000", "count": 23 },
    { "timePeriod": "201701310000", "count": 12 }
   ],
  "totalCount":2027, 
  "requestParameters":
    {
      "bucket":"day",
      "fromDate":"201701010000",
      "toDate":"201802010000"
    }
}
</pre> 

Note that Tweet count requests for very high volume queries may result in a 'next' token. You can continue to pass in the 'next' element from your previous query until you have received all counts from the query time period. When you receive a response that does not include a 'next' element, it means that you have reached the last page and no additional counts are available in your time range.

## HTTP response codes<a id="HTTPCodes" class="tall">&nbsp;</a>

<table class='table'>
  <tr>
    <th>
      Status</th>
    <th>
      Text</th>
    <th>
      Description</th>
  </tr>
  <tr>
    <td>
      200</td>
    <td>
      OK</td>
    <td>
      The request was successful. The JSON response will be similar to the following:
    </td>
  </tr>
  <tr>
    <td>
      400</td>
    <td>
       Bad Request</td>
    <td>
      Generally, this response occurs due to the presence of invalid JSON in the request, or where the request failed to send any JSON payload. <br />
     </td>
  </tr>
  <tr>
    <td>401</td>
    <td>Unauthorized</td>
    <td>
      HTTP authentication failed due to invalid credentials. <br />      
    </td>
  </tr>
  <tr>
    <td>404</td>
    <td>Not Found</td>
    <td>
      The resource was not found at the URL to which the request was sent, likely because an incorrect URL was used.
      </td>
  </tr>
  <tr>
    <td>
      422</td>
    <td>
      Unprocessable Entity</td>
    <td>
      This is returned due to invalid parameters in the query -- e.g. invalid PowerTrack rules.<br />
    </td>
  </tr>
  <tr>
    <td>
      429</td>
    <td>
      Unknown Code</td>
    <td>
      Your app has exceeded the limit on connection requests. The corresponding JSON message will look similar to the following:<br />
    </td>
  </tr>
  <tr>
    <td>
      500</td>
    <td>
      Internal Server Error</td>
    <td>
      There was an error on the server side. Retry your request using an exponential backoff pattern.<br />
    </td>
  </tr>
  <tr>
    <td>
      502</td>
    <td>
      Proxy Error</td>
    <td>
      There was an error on the server side. Retry your request using an exponential backoff pattern.<br />
    </td>
  </tr>
  <tr>
    <td>
      503</td>
    <td>
       Service Unavailable</td>
    <td>
        There was an error on the server side. Retry your request using an exponential backoff pattern.<br />
    </td>
    </tr>
</table>
