__Where did that tweet come from?__ Sometimes geographic metadata really matters.

The [“Tweeting in the Rain” blog series](http://blog.gnip.com/tweeting-in-the-rain/) discussed looking for a Twitter “signal” around local rain gauges. When attempting to correlate rain gauge data to Twitter communications, determining where the tweet came from matters. [Part 2] (http://blog.gnip.com/tweeting-in-the-rain-part-2/) of that series focused on a flood in Louisville, KY on August 4-5, 2009.  During that 48-hour period there were approximately 30,000,000 tweets from around the world.  By filtering with domain-specific keywords, we ended up with around 450,000 rain, flood,  and weather related tweets. Our next challenge was to determine which of these were from the Louisville, KY area.

There are generally three methods for geo-referencing Twitter data:

- Activity Location: tweets that are geo-tagged by the user.
 Specific location or a Twitter Place
- Profile Location: parsing the Twitter Account Profile location provided by the user.
“I live in Louisville, home of the Derby!”
- Mentioned Location: parsing the tweet message for geographic location.
“I’m in Louisville and it is raining cats and dogs”

__Why are Profile Locations important?__

Having a tweet explicitly tied to a specific location or a Twitter Place is extremely useful for any geographic analysis. However, the percentage of tweets with an Activity Location is less than 2%.  Across the ten events we looked into, here is how the geo-referencing was attempted:

- User account profile: 82%
- Tweet text: 17%
- Tweet geo-tagging: 1% 

By far the majority of geo-referencing was performed by parsing the words from the Profile location.  For the Louisville event we searched for any tweet that either mentioned “Louisville” in the tweet, or came from an Twitter account with a Profile Location setting including “Louisville.” It’s worth noting that since we live near Louisville, CO, we explicitly excluded account locations that mentioned “CO” or “Colorado.” 

We needed to build our own language-based filters to parse text for hints on specific locations.  Detecting location references in language is inherently a task that benefits from local knowledge.  Since we live near Louisville, CO, we knew we should try to explicitly avoid collecting any Colorado tweets.  While this local knowledge was helpful, we did not even try to filter out Louisville, Tennessee and the other eight (Alabama, Georgia, Illinois, Kansas, Mississippi, Nebraska, New York, and Ohio!). One reason was that this event was from 2009, when Twitter was in its relative infancy.  We figured that, compared four years later, there really were not that many tweeting, and those that were probably lived in Louisville Kentucky, not Tennessee.  If that was a weak assumption for back then, it is a much less safe assumption today.  So it was a calculated risk to simplify our “language processing” algorithm and just focus on the Louisvilles in Kentucky and Colorado only.


__PowerTrack Operators for Profile Location__


PowerTrack rules used for Louisville flood analysis:
bio_location_contains:”louisville” - (bio_location_contains:colorado OR bio_location_contains:co)

Optimized version with bio_location_contains: 
bio_location_contains:”louisville” 
- (bio_location_contains:colorado OR bio_location_contains:”, CO” 
OR bio_location_contains:ohio OR bio_location_contains:”, OH”
OR bio_location_contains:tennesee OR bio_location_contains:”, TN” OR
OR bio_location_contains:new york OR bio_location_contains:”, NY”) 

This version is not that bad in terms of length or complexity, but it potentially has serious drawbacks.  

It required extra research to determine what other states have a town called Louisville.  This rule does not cover the other six states with a Louisville.  For our use-case we didn't think this was a big deal since these were smaller towns and this Louisville flood occurred back in 2009 when Twitter was in its relative infancy.  Based on the results ([Tweeting in the Rain - Part 3] (http://blog.gnip.com/tweeting-in-the-rain-part-3/)) we saw it seems that these assumptions were reasonable. However, as the use of social networks grows the assumption of not much usage in certain areas becomes less safe. Fortunately there is an easier and more effective way to tackle this issue.


__Profile Location enrichments__

The beautiful thing about the new enhanced Profile Location tools is that it abstracts away advanced algorithms that blend geographic and language processing.  With these new tools, we could have simply specified the City of Louisville and the State of Kentucky and known that we’d optimized our results, knowing that we were easily omitting tweets from towns named Louisville in states other than Kentucky.

New rule with Profile Location operators:
profile_locality:louisville profile_region:kentucky

This rule is much shorter and more effective.  If we had had these new tools we likely would have done a better job at finding Twitter communications around where these gauges were located. 



