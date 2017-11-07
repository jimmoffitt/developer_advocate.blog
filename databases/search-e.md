---
layout: documentation_new
category: documentation
title: Enterprise search APIs
description: API reference for enterprise 30-Day and Full-Archive search APIs
heading: Enterprise search APIs
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
- name: Counts request examples 
  link: CountsRequestExamples  
- name: HTTP response codes
  link: HTTPCodes
---

## Introduction<a id="Introduction" class="extra-tall">&nbsp;</a>

There are two enterprise search APIs:

+ 30-Day Search API: provides Tweets posted with the last 30 days.
+ Full-Archive Search API: provides Tweets from the entire archive, starting with the first Tweet posted in March, 2006.

These search APIs share a common design and the documentation below applies to both. 

Below you will find important details needed when integrating with the enterprise search APIs:

+ Methods for requesting Tweet data and counts
+ Pagination
+ API request parameters and example requests
+ API response JSON payloads and example responses
+ HTTP response codes

## Methods<a id="Methods" class="extra-tall">&nbsp;</a>

The base URI for enterprise search is ```https://gnip-api.twitter.com/search/```.

<table class="table table-hover">
    <tr>
        <th>
            Method</th>
        <th>
            Description</th>
    </tr>
    <tr>
        <td>
            <a href="#SearchRequests">POST /search/:product/accounts/:account_name/:label</a></td>
        <td>
            Retrieve Tweets from the past 30 days that match the specified PowerTrack rule.</td>
    </tr>
    <tr>
        <td>
            <a href="#CountRequests">POST /search/:product/accounts/:account_name/:label/counts</a></td>
        <td>
            Retrieve the number of Tweets from the past 30 days that match the specified PowerTrack rule.</td>
    </tr>
</table>

Where:
 - ```:product``` indicates the search endpoint you are making requests to, either ```30day``` or ```fullarchive```.
 - ```:account_name``` is the (case-sensitive) name associated with your account, as displayed at console.gnip.com
 - ```:label``` is the (case-sensitive) label associated with your search endpoint, as displayed at console.gnip.com

For example, if the TwitterDev account has the 30-Day search product with a label of 'prod' (short for production), the search endpoints would be:

+ Data endpoint: https://gnip-api.twitter.com/search/30day/accounts/TwitterDev/prod.json
+ Counts endpoint: https://gnip-api.twitter.com/search/30day/accounts/TwitterDev/prod/counts.json

Your complete enterprise search API endpoint is displayed at https://console.gnip.com.

The example endpoints below will be based on the 30-Day search API, using a ```:product``` set to '30day'. If you are using Full-Archive search, simply use the 'fullarchive' product name.

## Authentication<a id="Authentication" class="tall">&nbsp;</a>

All requests to the enterprise search APIs must use HTTP *Basic Authentication*, constructed from a valid email address and password combination used to log into your account at https://console.gnip.com. Credentials must be passed as the *Authorization* header for each request.

## Request/response behavior<a id="RequestResponseBehavior" class="tall">&nbsp;</a>

Using the ```fromDate``` and ```toDate``` parameters, you can request any time period that the API supports. The 30-Day search API provides Tweets from the most recent 31 days (even though referred to as the '30-Day' API, it makes 31 days available to enable users to make complete-month requests). The Full-Archive search API provides Tweets back to the very first tweet (March 21, 2006).  However, a single response will be limited to the lesser of your specified 'maxResults' or 31 days. If matching data or your time range exceeds your specified maxResults or 31 days, you will receive a 'next' token which you should use to paginate through remainder of your specified time range.

For example, say you are using Full-Archive search and want all Tweets matching your query from January 1, 2017 to June 30, 2017. You will specify that full six-month time period in your request using the ```fromDate``` and ```toDate``` parameters.  The search API will respond with the first 'page' of Tweets, with the number of Tweets matching your ```maxResults``` parameter (which defaults to 100). Assuming there are more Tweets (and there most likely will be more), the API will also provide a 'next' token that enables you to make a request for the next 'page' of data. This process is repeated until the API does not return a 'next' token. See the next section for more details.

## Pagination<a id="Pagination" class="tall">&nbsp;</a>

When making both data and count requests it is likely that there is more data than can be returned in a single response. When that is the case the response will include a ‘next’ token. The ‘next’ token is provided as a root-level JSON attribute. Whenever a ‘next’ token is provided, there is additional data to retrieve so you will need to keep making API requests.

<b>Note:</b> The ‘next’ token behavior differs slightly for data and counts requests, and both are described below with example responses provided in the API Reference section.

### Data pagination

Requests for data will likely generate more data than can be returned in a single response. Each data request includes a parameter that sets the maximum number of Tweets to return per request. This ```maxResults``` parameter defaults to 100, and can be set to a range of 10-500. If your query matches more Tweets than the 'maxResults' parameter used in the request, the response will include a 'next' token (as a root-level JSON attribute). This 'next' token is used in the subsequent request to retrieve the next portion of the matching Tweets for that query (i.e. the next 'page”). Next tokens will continue to be provided until you have reached the last 'page' of results for that query, when no 'next' token is provided.

To request the next 'page' of data, you must make the exact same query as the original, including ```query```, ```toDate```, and ```fromDate``` parameters, if used, and also include a 'next' request parameter set to the value from the previous response. This can be utilized with either a GET or POST request. However, the 'next' parameter must be URL encoded in the case of a GET request.

You can continue to pass in the 'next' element from your previous query until you have received all Tweets from the time period covered by your query. When you receive a response that does not include a 'next' element, it means that you have reached the last page and no additional data is available for the specified query and time range.

### Counts pagination

The 'counts' endpoint provides Tweet volumes associated with a query on either a daily, hourly, or per-minute basis. The 'counts' API endpoint will return a timestamped array of counts for a maximum of 31-day payload of counts. If you request more than 31 days of counts, you will be provided a 'next' token. As with the data 'next' tokens, you must make the exact same query as the original and also include a 'next' request parameter set to the value from the previous response.

Beyond requesting more than 31 days of counts, there is another scenario when a 'next' token is provided. For higher volume queries, there is the potential that the generation of counts will take long enough to trigger a response timeout. When this occurs you will receive less than 31 days of counts, but will be provided a 'next' token in order to continue making requests for the entire payload of counts. <b>*Important:*</b> Timeouts will only issue full "buckets" - so 2.5 days would result in 2 full day "buckets".

### Additional notes
+ When using a fromDate or toDate in a search request, you will only get results from within your time range. When you reach the last group of results within your time range, you will not receive a next element.
+ The 'next' element can be used with any maxResults value between 10-500 (with a default value of 100). The maxResults determines how many Tweets are returned in each response, but does not prevent you from eventually getting all results.
+ The 'next' element does not expire. Multiple requests using the same 'next' query will receive the same results, regardless of when the request is made.
+ When paging through results using the 'next' parameter, you may encounter duplicates at the edges of the query. Your application should be tolerant of these.

## Data endpoint <a class='tall' id='DataEndpoint'>&nbsp;</a>

### POST /search/:label

#### Endpoint pattern: 
+ 30-Day search API: /30day/accounts/:account_name/:label.json
+ Full-Archive search API: /fullarchive/accounts/:account_name/:label.json

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
            The equivalent of one PowerTrack rule, with up to 2,048 characters (and no limits on the number of positive and negative clauses).<br />
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
        <th>tag</th>
        <td>Tags can be used to segregate rules and their matching data into different logical groups. If a rule tag is provided the rule tag is included in the 'matching_rules' attribute. <br /> 
          <br />
          It is recommended to assign rule-specific UUIDs to rule tags and maintain desired mappings on the client side. <br />
          <br />
        </td>
        <td>No</td>
        <td>8HYG54ZGTU</td>
    </tr>
    <tr>
        <th>fromDate</th>
        <td>The oldest UTC timestamp (back to 3/21/2006 with Full-Archive search) from which the activities will be provided. Timestamp is in minute granularity and is inclusive (i.e. 12:00 includes the 00 minute).<br />
        <br />
        <i>Specified:</i> Using only the fromDate with no toDate parameter will deliver results for the query going back in time from now( ) until the fromDate.<br />
        <br />
        <i>Not Specified:</i> If a fromDate is not specified, the API will deliver all of the results for 30 days prior to now( ) or the toDate (if specified).<br /> 
        <br />
        If neither the fromDate or toDate parameter is used, the API will deliver all results for the most recent 30 days, starting at the time of the request, going backwards.<br />
        </td>
        <td>No</td>
        <td>201207220000</td>
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
        <td>The maximum number of search results to be returned by a request. A number between 10 and the system limit (currently 500). By default, a request response will return 100 results.</td>
        <td>No</td>
        <td>500</td>
    </tr>
    <tr>
        <th>next</th>
        <td>This parameter is used to get the next "page" of results as described <a href="#Pagination">HERE</a>. The value used with the parameter is pulled directly from the response provided by the API, and should not be modified.</td>
        <td>No</td>
        <td>NTcxODIyMDMyODMwMjU1MTA0</td>
    </tr>
</table>
<br />

#### Additional details<a class='tall' id='AdditionalDetails'>&nbsp;</a>

<table class='table'>
    <tr>
        <td><strong>Available Timeframe</strong></td>
        <td>30-Day: last 31 days || Full-Archive: March 21, 2006 - Present</td>
    </tr>
    <tr>
        <td><strong>Query Format</strong></td>
        <td>The equivalent of one PowerTrack rule, with up to 2048 characters (and no limits on the number of positive and negative clauses). <br />
            <br />
            <strong>Items to Note:</strong>
            <ul>
              <li>Not all PowerTrack operators are supported. Supported Operators are listed <a href='/en/docs/tweets/search/overview/full-archive-search#AvailableOperators'>HERE</a>.</li> 
            </ul>
     </td>
    </tr>
    <tr>
        <td><strong>Rate Limit</strong></td>
        <td>Partners will be rate limited at both minute and second granularity. The per minute rate limit will vary by partner as specified in your contract. However, these per-minute rate limits are not intended to be used in a single burst. Regardless of your per minute rate limit, all partners will be limited to a maximum of 20 requests per second, aggregated across all requests for data and/or counts.</td>
    </tr>
    <tr>
        <td><strong>Compliance</strong></td>
        <td>All data delivered via the Full-Archive Search API is compliant at the time of delivery.</td>
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
curl -X POST -u&lt;username&gt; "https://gnip-api.twitter.com/search/:product/accounts/:account_name/:label.json" -d '{"query":"TwitterDev \"search api\"","maxResults":"500","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;"}'
</pre>

If the API data response includes a 'next' token, below is a subsequent request that consists of the original request, with the 'next' parameter set to the provided token:

<pre>
curl -X POST -u&lt;username&gt; "https://gnip-api.twitter.com/search/:product/accounts/:account_name/:label.json" -d '{"query":"TwitterDev \"search api\"","maxResults":"500","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;",
"next":"NTcxODIyMDMyODMwMjU1MTA0"}'
</pre>

#### Example GET request<a class='tall' id='SearchGetRequest'>&nbsp;</a>

- Request parameters in a GET request are encoded into the URL, using standard URL encoding.
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter.
- Do not split portions of the rule out as separate parameters in the query URL.

Here is an example GET (using cURL) command for making an initial data request:

<pre>
curl -u&lt;username&gt; "http://gnip-api.twitter.com/search/:product/accounts/:account_name/:label.json?query=TwitterDev%20%5C%22search%20api%5C%22&amp;maxResults=500&amp;fromDate=&lt;yyyymmddhhmm&gt;&amp;toDate=&lt;yyyymmddhhmm&gt;"
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
        "fromDate":"201101010000",
        "toDate":"201201010000"
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

### /search/:stream/counts<a class='tall' id='CountRequests'>&nbsp;</a>

#### Endpoint pattern: 
/fullarchive/accounts/:account_name/:label/counts.json

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
            The equivalent of one PowerTrack rule, with up to 2,048 characters (and no limits on the number of positive and negative clauses).<br />
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
        <td>The oldest UTC timestamp (back to 3/21/2006) from which the activities will be provided. Timestamp is in minute granularity and is inclusive (i.e. 12:00 includes the 00 minute).<br />
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
        <td>The unit of time for which count data will be provided. Count data can be returned for every day, hour or minute in the requested timeframe. By default, hourly counts will be provided. Options: 'day', 'hour', 'minute'
        </td>
        <td>No</td>
        <td>minute</td>
    </tr>
    <tr>
        <th>next</th>
        <td>This parameter is used to get the next 'page' of results as described <a href="#Pagination">HERE</a>. The value used with the parameter is pulled directly from the response provided by the API, and should not be modified.</td>
        <td>No</td>
        <td>NTcxODIyMDMyODMwMjU1MTA0</td>
    </tr>
</table>

#### Additional details

<table class='table'>
    <tr>
        <td><strong>Available Timeframe</strong></td>
        <td>30-Day: last 31 days || Full-Archive: March 21, 2006 - Present</td>
    </tr>
    <tr>
        <td><strong>Query Format</strong></td>
        <td>The equivalent of one PowerTrack rule, with up to 2048 characters (and no limits on the number of positive and negative clauses). <br />
            <br />
            <strong>Items to Note:</strong>
            <ul>
              <li>Not all PowerTrack operators are supported. Supported Operators are listed <a href='/en/docs/tweets/search/overview/full-archive-search#AvailableOperators'>HERE</a>.</li> 
            </ul>
        </td>
    </tr>
    <tr>
        <td><strong>Rate Limit</strong></td>
        <td>Partners will be rate limited at both minute and second granularity. The per minute rate limit will vary by partner as specified in your contract. However, these per-minute rate limits are not intended to be used in a single burst. Regardless of your per minute rate limit, all partners will be limited to a maximum of 20 requests per second, aggregated across all requests for data and/or counts.</td>
    </tr>
    <tr>
        <td><strong>Count Precision</strong></td>
        <td>The counts delivered through this endpoint reflect the number of activities that occurred and do not reflect any later compliance events (deletions, scrub geos). Some activities counted may not be available via data endpoint due to user compliance actions.</td>
    </tr>
</table>

### Example counts requests and responses<a class='tall' id='CountsRequestExamples'>&nbsp;</a>

#### Example POST request

- Request parameters in a POST request are sent via a JSON-formatted body, as shown below.
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter.
- Do not split portions of the rule out as separate parameters in the query URL.

Here is an example POST (using cURL) command for making an initial counts request:

<pre>
curl -X POST -u&lt;username&gt; "https://gnip-api.twitter.com/search/fullarchive/accounts/:account_name/:label/counts.json" -d '{"query":"TwitterDev","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;","bucket":"day"}'
</pre>

If the API counts response includes a 'next' token, below is a subsequent request that consists of the original request, with the 'next' parameter set to the provided token:

<pre>
curl -X POST -u&lt;username&gt; "https://gnip-api.twitter.com/search/fullarchive/accounts/:account_name/:label/counts.json" -d '{"query":"TwitterDev","fromDate":"&lt;yyyymmddhhmm&gt;","toDate":"&lt;yyyymmddhhmm&gt;","bucket":"day",
"next":"YUcxO87yMDMyODMwMjU1MTA0"}'
</pre>

#### Example GET request

- Request parameters in a GET request are encoded into the URL, using standard URL encoding
- All portions of the PowerTrack rule being queried for (e.g. keywords, other operators like bounding_box:) should be placed in the 'query' parameter
- Do not split portions of the rule out as separate parameters in the query URL

Here is an example GET (using cURL) command for making an initial counts request:

<pre>
curl -u&lt;username&gt; "http://gnip-api.twitter.com/search/fullarchive/accounts/:account_name/:label/counts.json?query=TwitterDev&amp;bucket=day&amp;fromDate=&lt;yyyymmddhhmm&gt;&amp;toDate=&lt;yyyymmddhhmm&gt;"
</pre>

#### Example counts responses

Below is an example response to a counts (data volume) query. This example response includes a next token, meaning the counts request was for more than 31 days, or that the submitted query had a large enough volume associated with it to trigger a partial response. 

The value of the 'next' element will change with each query and should be treated as an opaque string. The 'next' element will look like the following in the response body:

<pre>
{
  "results": [
    { "timePeriod": "201101010000", "count": 32 },
    { "timePeriod": "201101020000", "count": 45 },
    { "timePeriod": "201101030000", "count": 57 },
    { "timePeriod": "201101040000", "count": 123 },
    { "timePeriod": "201101050000", "count": 134 },
    { "timePeriod": "201101060000", "count": 120 },
    { "timePeriod": "201101070000", "count": 43 },
    { "timePeriod": "201101080000", "count": 65 },
    { "timePeriod": "201101090000", "count": 85 },
    { "timePeriod": "201101100000", "count": 32 },
    { "timePeriod": "201101110000", "count": 23 },
    { "timePeriod": "201101120000", "count": 85 },
    { "timePeriod": "201101130000", "count": 32 },
    { "timePeriod": "201101140000", "count": 95 },
    { "timePeriod": "201101150000", "count": 109 },
    { "timePeriod": "201101160000", "count": 34 },
    { "timePeriod": "201101170000", "count": 74 },
    { "timePeriod": "201101180000", "count": 24 },
    { "timePeriod": "201101190000", "count": 90 },
    { "timePeriod": "201101200000", "count": 85 },
    { "timePeriod": "201101210000", "count": 93 },
    { "timePeriod": "201101220000", "count": 48 },
    { "timePeriod": "201101230000", "count": 37 },
    { "timePeriod": "201101240000", "count": 54 },
    { "timePeriod": "201101250000", "count": 52 },
    { "timePeriod": "201101260000", "count": 84 },
    { "timePeriod": "201101270000", "count": 120 },
    { "timePeriod": "201101280000", "count": 34 },
    { "timePeriod": "201101290000", "count": 83 },
    { "timePeriod": "201101300000", "count": 23 },
    { "timePeriod": "201101310000", "count": 12 }
   ],
  "totalCount":2027, 
  "next":"NTcxODIyMDMyODMwMjU1MTA0",
  "requestParameters":
    {
      "bucket":"day",
      "fromDate":"201101010000",
      "toDate":"201201010000"
    }
}
</pre> 

The response to a subsequent request might look like the following (note the new counts timeline and different 'next' value):

<pre>
{
  "results": [
    { "timePeriod": "201102010000", "count": 45 },
    { "timePeriod": "201102020000", "count": 76 },
     ....
    { "timePeriod": "201103030000", "count": 13 }
 ],
 "totalCount":3288, 
 "next":"WE79fnakFanyMDMyODMwMjU1MTA0",
 "requestParameters":
    {
      "bucket":"day",
      "fromDate":"201101010000",
      "toDate":"201201010000"
    }
}

</pre>

You can continue to pass in the 'next' element from your previous query until you have received all counts from the query time period. When you receive a response that does not include a 'next' element, it means that you have reached the last page and no additional counts are available in your time range.

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
      HTTP authentication failed due to invalid credentials. Log in to console.gnip.com with your credentials to ensure you are using them correctly with your request. <br />      
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
      There was an error on server side. Retry your request using an exponential backoff pattern.<br />
    </td>
  </tr>
  <tr>
    <td>
      503</td>
    <td>
       Service Unavailable</td>
    <td>
        There was an error on server side. Retry your request using an exponential backoff pattern.<br />
    </td>
    </tr>
</table>
