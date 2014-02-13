# Twitter Geo-Referencing: An Example Use-case
####Twitter signals around local rain events

The [Tweeting in the Rain] (http://blog.gnip.com/tweeting-in-the-rain/) blog series discussed looking for a Twitter “signal" around rain gauges in six areas of the US including San Diego, Las Vegas, Boulder and Louisville. Rain gauge data was compiled from ten significant storms and compared with hourly volumes of Twitter traffic.

Geo-referencing Twitter data was fundamental to this research. Part 2 of that series focused on a flood in Louisville, KY on August 4-5, 2009. During that 48-hour period there were approximately 30,000,000 tweets from around the world. By filtering with domain-specific keywords, approximately 450,000 rain, flood, and weather related tweets were identified. The next challenge was to determine which of these were from the Louisville, KY area.

The first step was building filters that searched for mentions of our areas of interest in the tweet content and Profile Location. Geo-tagged tweets are not available prior to 2011-01-01 for Twitter Compliance reasons, so no Activity Location metadata was available for this 2009 Louisville rain event.

Across the ten rain events analyzed, here is the breakdown of the geographic metadata available for geo-referencing tweets:
+ Profile Location: 82%
+ Mentioned Location: 17%
+ Activity Location: 1%

By far the majority of geo-referencing was performed by parsing the words from the Profile Location. These numbers help illustrate the importance of Profile Locations when attempting to determine where tweets come from. Next the power of the Profile Location enrichment will be demonstrated by comparing filtering with and without Profile Geo metadata.


###Life before the Profile Geo Enrichment

Data for the study was collected and analyzed in February 2013, before the Profile Geo beta enrichment was released. Before the Profile Geo enrichment there were two PowerTrack Operators available for filtering on Profile Locations:
+ bio_location
+ bio_location_contains

These operators work by looking for a tokenized match and substring match in the contents of the Twitter user’s Profile Location setting. For the Louisville event tweet contents and Profile Locations were searched for any mention of  “Louisville.”

Using the “bio_location” PowerTrack Operators, we used the following clause for geo-referencing Profile Location metadata:

     bio_location:louisville -(bio_location:colorado OR bio_location_contains:" , CO")

These two Operators were used to build language-based filters to identify tweets associated with Profile Locations that mention areas of interest. The negation clause of the rule was driven by local geographic knowledge. Gnip is headquarter in Boulder, CO, which is about ten miles from the City of Louisville, with a population of 20,000. Accordingly, the negation clause was added to prevent Colorado tweets from ‘leaking; into our data set focused on Louisville, Kentucky. However, there was no attempt to filter out tweets from Louisville, Tennessee and the other eight towns with that name in the States of Alabama, Georgia, Illinois, Kansas, Mississippi, Nebraska, New York, and Ohio!  We made the assumption that during the 48 hours of rain and flooding in 2009 (when Twitter was in its relative infancy), the vast majority of Profile Locations mentioning Louisville were from Kentucky.

However, if the assumption had been that tweets from those other geographic areas could be significant, many more location tokens would have needed. To make sure tweets from other locations were not ‘leaking’ into our data set, the complete rule would have looked like this:

```
   bio_location:"louisville"
     -(bio_location:colorado OR bio_location_contains:", CO" OR bio_location:ohio OR
     bio_location_contains:", OH" OR bio_location:tennessee OR      
     bio_location_contains:", TN" OR bio_location:"new york" OR bio_location_contains:", NY" 
     OR bio_location:kansas OR bio_location_contains:", KS" OR bio_location:alabama OR
     bio_location_contains:", AL" OR bio_location:georgia OR bio_location_contains:", GA" OR
     bio_location:illinois OR  bio_location_contains:", IL" OR bio_location:mississippi OR    
     bio_location_contains:", MS" OR bio_location:nebraska OR bio_location_contains:", NE")
     
```

This rule syntax is not very complex, but it is a bit long. More importantly, it depended on additional effort to determine what other states have a town called Louisville. Furthermore, it assumes that Twitter Profile Locations contained a very specific state abbreviation format (such as “, KS”), and does not cover other possibilities (such as “, Kan.”).

Since we saw good correlations between Louisville rainfall and our attempts to geo-reference tweets from that area, we choose not to further refine our rules with respect to Profile Locations. However, as the use of social networks grows the chances of collecting “false positives” increases and such assumptions become more suspect.
Fortunately there is an easier and more effective way to tackle this issue.

###Easier filtering with the Profile Geo enrichment
A powerful thing about the new enhanced Profile Location tools is that it abstracts away techniques that blend geographic and language processing. With these new tools, we could have simply specified the City of Louisville and the State of Kentucky and known that we’d optimized our results.  With the following simple rule we would have known that we were omitting tweets from towns named Louisville in states other than Kentucky:

    profile_locality:louisville profile_region:kentucky

This rule is much shorter and more effective.  If the Profile Geo enrichment had been available when we collected our data we likely would have done a better job at finding Twitter communications around where these rain gauges were located.
For more information on the power of the Gnip Profile Geo enrichment see these blog posts:

+ [Get More Twitter Geodata From Gnip With Our New Profile Geo Enrichment] (http://blog.gnip.com/twitter-geo-data-enrichment/)
+ [Profile Geo: When You Need More Geodata In Your Twitter Data] (http://blog.gnip.com/standard-geo-vs-profile-geo/)

For more details on working with Twitter geographic metadata see [this article](http://support.gnip.com/documentation/geo-intro.html).
