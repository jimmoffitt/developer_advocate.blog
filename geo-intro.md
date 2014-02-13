#Twitter Geographical Metadata

###Introduction
Many use-cases of Twitter data are based on determining generally where a tweet came from.  In addition, it seems nearly all use-cases have some geographical aspects that would be interesting to explore. Maybe you are interested in public opinion on health care legislation, or want to track customer satisfaction in different regions.
Perhaps you want to research social media communications during extreme weather.

There are many challenges in geo-referencing tweets. One is that there are different types of geographical metadata, with a range of ease-of-use. Probably the easiest form to work with are latitude and longitude coordinates in decimal degrees. This type of metadata is ‘ready-to-use’ for geographical analyses.  On the other extreme are mentions of locations and places, which require text parsing and analysis to extract.  

Another important consideration of the precision that the geographical metadata provides. In some cases an exact location is provided, while in other cases the precision is at the neighborhood, town, city or regional level. Meanwhile, by far the largest source of metadata is provided at the country level. So it is important to think about what precision your social media use-case depends on. For use-cases such as brand-monitoring, geo-referencing tweets at the regional level may be just right.  For other use-cases, such as research in economics and sociology, knowing what country a tweet came from may be all that matters. Other use-cases may depend on sourcing data from specific towns and cities.

###What geographical metadata comes with a tweet?

Twitter provides an option to ‘geo-tag’ tweets.  This geo-tagging can be based on an exact location, or assigned to a Twitter Place (see [HERE](https://blog.twitter.com/2010/twitter-places-more-context-your-tweets) and [HERE](https://dev.twitter.com/docs/platform-objects/places) for more information).  Twitter Places can be thought of as at the neighborhood level, which provides a “bounding box” with latitude and longitude coordinates that define the location area. This type of geographic metadata, referred to as an “Activity Location” provides the highest level of precision. Activity Locations require no language parsing/processing to access the geographic information.  The main drawback to relying on Activity Locations is that only 1-2% of tweets are geo-tagged.

A second source of geographic metadata are mentions of locations in the tweet content. This type of “Mentioned Location” metadata requires parsing the tweet message for location names of interest, including nicknames. One tweet may mention Manhattan, while another may mention the Big Apple.  

Finally, every Twitter Profile has a “Location” setting that can be filled out by the account owner.  These Profile Locations provide the largest source of geographic metadata. Not everyone provides this information, and it can contain any phrase the user wants. One Twitter account could have its location set to “Living in the Colorado foothills”, while another could be set to a less helpful “My parents’ basement.”

In summary, there are three metadata sources for geo-referencing tweets:
+ <b>Activity Location</b>: tweets that are geotagged with an exact location or Twitter Place.
    + Exact location with lat/long coordinates: [-85,7629,38.2267]
    + Twitter Place with a name (“Louisville Central”) and four pairs of lat/long coordinates that define a “bounding box.” 
+ <b>Mentioned Location</b>: parsing the tweet message for geographic location.
    + “If you are in Louisville, check out the pizza place off main”
    + “I’m in Louisville and it is raining cats and dogs”
+ <b>Profile Location</b>: parsing the account-level location for locations of interest.
    + “I live in Louisville, home of the Derby!”
    + “I live in Louisville, the one in beautiful Colorado.”

For example JSON that illustrates how this metadata is delivered in the tweet payload, along with details on how to filter on it, see this article.

###How can I use this metadata to geo-reference tweets?

Gnip PowerTrack provides many ways to filter on these types of geographic metadata. These filters, or rules, are built using the more than fifty PowerTrack Operators (see complete documentation [HERE](http://support.gnip.com/sources/twitter.html#PowerTrackOperators)).

See our [Filtering Twitter by Location](http://support.gnip.com/documentation/filtering_twitter_by_location.html) article for an introduction to the PowerTrack Operators that can be used to filter on Activity Locations and Profile Locations. Since Profile Locations are by far the largest source of Twitter geographic metadata, Gnip provides the Profile Geo enrichment. 

Since Profile Geo vastly increases the amount of geographic data, there has been quick and wide adoption of this enrichment. For introductions to the power of the Gnip Profile Geo data enrichment see these blog posts:
+ [Get More Twitter Geodata From Gnip With Our New Profile Geo Enrichment](http://blog.gnip.com/twitter-geo-data-enrichment/)
+ [Profile Geo: When You Need More Geodata In Your Twitter Data](http://blog.gnip.com/standard-geo-vs-profile-geo/)

Finally, to help illustrate the power of the Profile Geo enrichment, this article details a research project that depended on successfully geo-referencing Twitter data.  
