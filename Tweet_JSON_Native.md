  
## Tweet Data Dictionary <a id="tweet" class="tall">&nbsp;</a>

Tweets are the basic atomic building block of all things Twitter. Tweets are also known as “status updates.” 

Tweet object:


```
{
 "id":
 "created_at":
 "text":
 "user": 
 "entities":
 "extended_entities":
}
```

<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead valign="bottom">
<tr class="row-odd"><th class="head">Attribute</th>
<th class="head">Type</th>
<th class="head">Description</th>
</tr>
</thead>
<tbody valign="top">

<tr class="row-even"><td>created_at</td>
<td>String</td>
<td><p class="first">UTC time when this Tweet was created.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;created_at&quot;:&quot;Wed Aug 27 13:08:45 +0000 2008&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>id</td>
<td>Int64</td>
<td><p class="first">The integer representation of the unique identifier for this Tweet. This number is greater than 53 bits and some programming
languages may have difficulty/silent defects in interpreting it. Using a signed 64 bit integer for storing this identifier is safe.
Use      <code class="docutils literal"><span class="pre">id_str</span></code>     for fetching the identifier to stay on the safe side. See <a class="reference external" href="https://dev.twitter.com/overview/api/twitter-ids-json-and-snowflake">Twitter IDs, JSON and
Snowflake</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id&quot;:114749583439036416
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>id_str</td>
<td>String</td>
<td><p class="first">The string representation of the unique identifier for this Tweet. Implementations should use this rather than the large integer in
<code class="docutils literal"><span class="pre">id</span></code>.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id_str&quot;:&quot;114749583439036416&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>text</td>
<td>String</td>
<td><p class="first">The actual UTF-8 text of the status update. See
<a class="reference external" href="https://github.com/twitter/twitter-text/blob/master/rb/lib/twitter-text/regex.rb">twitter-text</a> for details on what is currently considered valid characters.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;text&quot;:&quot;Tweet Button, Follow Button, and Web Intents javascript now support SSL http:\/\/t.co\/9fbA0oYy ^TS&quot;
</pre></div>

</div>
</td>
</tr>

<tr class="row-even"><td>source</td>
<td>String</td>
<td><p class="first">Utility used to post the Tweet, as an HTML-formatted string. Tweets from the Twitter website have a source value of <code class="docutils literal"><span class="pre">web</span></code>.</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;source&quot;:&quot;\u003Ca href=\&quot;http:\/\/itunes.apple.com\/us\/app\/twitter\/id409789998?mt=12\&quot; \u003ETwitter for Mac\u003C\/a\u003E&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>truncated</td>
<td>Boolean</td>
<td><p class="first">Indicates whether the value of the      <code class="docutils literal"><span class="pre">text</span></code>     parameter was truncated, for example, as a result of a retweet exceeding the 140
character Tweet length. Truncated text will end in ellipsis, like this      <code class="docutils literal"><span class="pre">...</span></code>     Since Twitter now rejects long Tweets vs
truncating them, the large majority of Tweets will have this set to      <code class="docutils literal"><span class="pre">false</span></code>     .
Note that while native retweets may have their toplevel      <code class="docutils literal"><span class="pre">text</span></code>     property shortened, the original text will be available
under the      <code class="docutils literal"><span class="pre">retweeted_status</span></code>     object and the      <code class="docutils literal"><span class="pre">truncated</span></code>     parameter will be set to the value of the original
status (in most cases,      <code class="docutils literal"><span class="pre">false</span></code>     ).
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;truncated&quot;:true
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>in_reply_to_status_id</td>
<td>Int64</td>
<td><p class="first"><em>Nullable.</em> If the represented Tweet is a reply, this field will contain the integer representation of the original Tweet&#8217;s ID.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;in_reply_to_status_id&quot;:114749583439036416
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>in_reply_to_status_id_str</td>
<td>String</td>
<td><p class="first"><em>Nullable.</em> If the represented Tweet is a reply, this field will contain the string representation of the original Tweet&#8217;s ID.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;in_reply_to_status_id_str&quot;:&quot;114749583439036416&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>in_reply_to_user_id</td>
<td>Int64</td>
<td><p class="first"><em>Nullable.</em> If the represented Tweet is a reply, this field will contain the integer representation of the original Tweet&#8217;s author
ID. This will not necessarily always be the user directly mentioned in the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;in_reply_to_user_id&quot;:819797
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>in_reply_to_user_id_str</td>
<td>String</td>
<td><p class="first"><em>Nullable.</em> If the represented Tweet is a reply, this field will contain the string representation of the original Tweet&#8217;s author ID.
This will not necessarily always be the user directly mentioned in the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;in_reply_to_user_id_str&quot;:&quot;819797&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>in_reply_to_screen_name</td>
<td>String</td>
<td><p class="first"><em>Nullable.</em> If the represented Tweet is a reply, this field will contain the screen name of the original Tweet&#8217;s author.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;in_reply_to_screen_name&quot;:&quot;twitterapi&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>user</td>
<td><a class="reference external" href="https://github.com/jimmoffitt/developer_advocate.blog/blob/master/User_JSON_Native.md">User object</a></td>
<td><p class="first">The user who posted this Tweet. See User data dictionary for complete list of attributes.</p>
<p>Example highlighting select attributes:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>
{&#10;  &#34;user&#34;: {&#10;    &#34;id&#34;: 2244994945,&#10;    &#34;id_str&#34;: &#34;2244994945&#34;,&#10;    &#34;name&#34;: &#34;TwitterDev&#34;,&#10;    &#34;screen_name&#34;: &#34;TwitterDev&#34;,&#10;    &#34;location&#34;: &#34;Internet&#34;,&#10;    &#34;url&#34;: &#34;https:\/\/dev.twitter.com\/&#34;,&#10;    &#34;description&#34;: &#34;Your official source for Twitter Platform news, updates &amp; events. Need technical help? Visit https:\/\/twittercommunity.com\/ \u2328\ufe0f #TapIntoTwitter&#34;,&#10;    &#34;verified&#34;: true,&#10;    &#34;followers_count&#34;: 477684,&#10;    &#34;friends_count&#34;: 1524,&#10;    &#34;listed_count&#34;: 1184,&#10;    &#34;favourites_count&#34;: 2151,&#10;    &#34;statuses_count&#34;: 3121,&#10;    &#34;created_at&#34;: &#34;Sat Dec 14 04:35:55 +0000 2013&#34;,&#10;    &#34;utc_offset&#34;: -25200,&#10;    &#34;time_zone&#34;: &#34;Pacific Time (US &amp; Canada)&#34;,&#10;    &#34;geo_enabled&#34;: true,&#10;    &#34;lang&#34;: &#34;en&#34;,&#10;    &#34;profile_image_url_https&#34;: &#34;https:\/\/pbs.twimg.com\/profile_images\/530814764687949824\/npQQVkq8_normal.png&#34;&#10;  }&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>geo</td>
<td>Object</td>
<td><strong>Deprecated.</strong> <em>Nullable.</em> Use the <code class="docutils literal"><span class="pre">coordinates</span></code> field instead. This deprecated attribute has its coordinates formatted as <em>[lat, long]</em>, while all other Tweet geo is formatted as <em>[long, lat]</em>. </td>
</tr>

<tr class="row-odd"><td>coordinates</td>
<td><a class="reference external" href="#obj-coordinates">Coordinates</a></td>
<td><p class="first"><em>Nullable.</em> Represents the geographic location of this Tweet as reported by the user or client application. The inner coordinates
array is formatted as <a class="reference external" href="http://www.geojson.org/">geoJSON</a> (longitude first, then latitude).
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;coordinates&quot;:
{
    &quot;coordinates&quot;:
    [
        -75.14310264,
        40.05701649
    ],
    &quot;type&quot;:&quot;Point&quot;
}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>place</td>
<td><a class="reference external" href="/overview/api/places">Places</a></td>
<td><p class="first"><em>Nullable</em> When present, indicates that the tweet is associated (but not necessarily originating from) a
<a class="reference external" href="/overview/api/places">Place</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;place&quot;:
{
    &quot;attributes&quot;:{},
     &quot;bounding_box&quot;:
    {
        &quot;coordinates&quot;:
        [[
                [-77.119759,38.791645],
                [-76.909393,38.791645],
                [-76.909393,38.995548],
                [-77.119759,38.995548]
        ]],
        &quot;type&quot;:&quot;Polygon&quot;
    },
     &quot;country&quot;:&quot;United States&quot;,
     &quot;country_code&quot;:&quot;US&quot;,
     &quot;full_name&quot;:&quot;Washington, DC&quot;,
     &quot;id&quot;:&quot;01fbe706f872cb32&quot;,
     &quot;name&quot;:&quot;Washington&quot;,
     &quot;place_type&quot;:&quot;city&quot;,
     &quot;url&quot;: &quot;http://api.twitter.com/1/geo/id/01fbe706f872cb32.json&quot;
}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>quoted_status_id</td>
<td>Int64</td>
<td><p class="first">This field only surfaces when the Tweet is a quote Tweet. This field contains the integer value Tweet ID of the quoted Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;quoted_status_id&quot;:114749583439036416
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>quoted_status_id_str</td>
<td>String</td>
<td><p class="first">This field only surfaces when the Tweet is a quote Tweet. This is the string representation Tweet ID of the quoted Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;quoted_status_id_str&quot;:&quot;114749583439036416&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>is_quote_status</td>
<td>Boolean</td>
<td><p class="first">Indicates whether this is a Quoted Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;is_quote_status&quot;:false
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>quoted_status</td>
<td><a class="reference external" href="/overview/api/tweets">Tweet</a></td>
<td>This field only surfaces when the Tweet is a quote Tweet. This attribute contains the Tweet object of the original Tweet that was
quoted.</td>
</tr>

<tr class="row-odd"><td>retweeted_status</td>
<td><a class="reference external" href="/overview/api/tweets">Tweet</a></td>
<td>Users can amplify the broadcast of Tweets authored by other users by <a class="reference external" href="/rest/reference/post/statuses/retweet/%3Aid">retweeting</a> .
Retweets can be distinguished from typical Tweets by the existence of a      <code class="docutils literal"><span class="pre">retweeted_status</span></code>     attribute. This attribute
contains a representation of the <em>original</em> Tweet that was retweeted. Note that retweets of retweets do not show representations of
the intermediary retweet, but only the original Tweet. (Users can also <a class="reference external" href="/rest/reference/post/statuses/destroy/%3Aid">unretweet</a> a
retweet they created by deleting their retweet.)</td>
</tr>

<tr class="row-odd"><td>quote_count</td>
<td>Integer</td>
<td><p class="first"><em>Nullable.</em> Indicates approximately how many times this Tweet has been quoted by Twitter users.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;quote_count&quot;:1138
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>reply_count</td>
<td>Int</td>
<td><p class="first">Number of times this Tweet has been replied to.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;reply_count&quot;:1585
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>retweet_count</td>
<td>Int</td>
<td><p class="first">Number of times this Tweet has been retweeted.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;retweet_count&quot;:1585
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>favorite_count</td>
<td>Integer</td>
<td><p class="first"><em>Nullable.</em> Indicates approximately how many times this Tweet has been  <a class="reference external" href="/rest/reference/post/favorites/create">liked</a>  by
Twitter users.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;favorite_count&quot;:1138
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>entities</td>
<td><a class="reference external" href="/overview/api/entities">Entities</a></td>
<td><p class="first">Entities which have been parsed out of the text of the Tweet. Additionally see <a class="reference external" href="/overview/api/entities-in-twitter-objects">Entities in Twitter
Objects</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;entities&quot;:
{
    &quot;hashtags&quot;:[],
    &quot;urls&quot;:[],
    &quot;user_mentions&quot;:[],
    &quot;media&quot;:[],
    &quot;symbols&quot;:[]
    &quot;polls&quot;:[]
}
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>extended_entities</td>
<td><a class="reference external" href="/overview/api/entities">Extended Entities</a></td>
<td><p class="first">When between one and four native photos or one video or one animated GIF are in Tweet, contains an array 'media' metadata. Additionally see <a class="reference external" href="/overview/api/entities-in-twitter-objects">Entities in Twitter
Objects</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;entities&quot;:
{
    &quot;media&quot;:[]
}
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>favorited</td>
<td>Boolean</td>
<td><p class="first"><em>Nullable.</em> Indicates whether this Tweet has been liked by the authenticating user.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;favorited&quot;:true
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>retweeted</td>
<td>Boolean</td>
<td><p class="first">Indicates whether this Tweet has been Retweeted by the authenticating user.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;retweeted&quot;:false
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>possibly_sensitive</td>
<td>Boolean</td>
<td><p class="first"><em>Nullable.</em> This field only surfaces when a Tweet contains a link. The meaning of the field doesn&#8217;t pertain to the Tweet content itself, but instead it is an indicator that the URL contained in the Tweet may contain content or media identified as sensitive content.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;possibly_sensitive&quot;:true
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>filter_level</td>
<td>String</td>
<td><p class="first">Indicates the maximum value of the <a class="reference external" href="/streaming/overview/request-parameters#filter_level">filter_level</a> parameter which may be
used and still stream this Tweet. So a value of <code class="docutils literal"><span class="pre">medium</span></code> will be streamed on <code class="docutils literal"><span class="pre">none</span></code>, <code class="docutils literal"><span class="pre">low</span></code>,
and <code class="docutils literal"><span class="pre">medium</span></code> streams.</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;filter_level&quot;: &quot;medium&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>lang</td>
<td>String</td>
<td><p class="first"><em>Nullable.</em> When present, indicates a <a class="reference external" href="http://tools.ietf.org/html/bcp47">BCP 47</a> language identifier corresponding to the machine-detected language of the Tweet text, or <code class="docutils literal"><span class="pre">und</span></code> if no language could be detected. See more documentation <a class="reference external" href="http://support.gnip.com/apis/powertrack2.0/rules.html#Operators">HERE</a>.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;lang&quot;: &quot;en&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>matching_rules</td>
<td>Array of Rule Objects</td>
<td><p class="first">Present in <em>filtered</em> products such as Twitter Search and PowerTrack. Provides the <em>id</em> and <em>tag</em> associated with the rule that matched the Tweet. With PowerTrack, more than one rule can match a Tweet. See more documentation <a class="reference external" href="http://support.gnip.com/enrichments/matching_rules.html">HERE</a>.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;matching_rules&quot;: &quot; [{
		&quot;tag&quot;: &quot;rain Tweets&quot;,
		&quot;id&quot;: 831566737246023680
	}, {
		&quot;tag&quot;: &quot;snow Tweet&quot;,
		&quot;id&quot;: 831567402366218240
	}]&quot;
</pre></div>
</div>
</td>
</tr>

</tbody>
</table>


## Next Steps

Explore the other sub-objects that a Tweet contains:

+ User objects are described HERE.
+ Entities and Extended Entitites objects are described HERE.
+ Tweet geo objects are described HERE.


  
