
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
