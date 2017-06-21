[] TODOS
+ [X] Add symbols
+ [X] Add polling metadata
+ [] Add relevant Operators? 
+ [X] Add Video extended entities details
+ [X] JSON pretty print encoding: https://opinionatedgeek.com/Codecs/HtmlEncoder (use checkbox!)

## Sections

+ [Twitter Entities](#entities)
+ [Twitter Extended Entities](#extended-entities)
+ [Entities Data Dictionary](#entities-data-dictionary)
+ [Extended Entities Data Dictionary](#extended-entities-data-dictionary)
+ [Example Native JSON](#example-json)

## Twitter Entities <a id="entities" class="tall">&nbsp;</a>

Entities provide metadata and additional contextual information about content posted on Twitter. The ```entities``` section provides  arrays of common things included in Tweets: hashtags, user mentions, links, stock tickers (symbols), Twitter polls, and attached media. These arrays are convenient for developers when ingesting Tweets, since Twitter has essentially pre-processed, or pre-parsed, the text body. Instead of needing to explicitly search and find these entities in the Tweet body, your parser can go straight to this JSON section and there they are.

Beyond providing parsing conveniences, the ```entities``` section also provides useful 'value-add' metadata. For example, if you are using the [Enhanced URLs enrichment](http://support.gnip.com/enrichments/enhanced_urls.html), URL metadata include fully-expanded URLs, as well as associated website titles and descriptions. Another example is when there are user mentions, the entities metadata include the numeric user ID, which are useful when making requests to many Twitter APIs. 

Every Tweet JSON payload includes an ```entities``` section, with the minimum set of ```hashtags```, ```urls```, ```user_mentions```, and ```symbols``` attributes, even if none of those entities are part of the Tweet message. For example, if you examine the JSON for a Tweet with a body of "Hello World!" and no attached media, the Tweet's JSON will include the following content with entity arrays containing  zero items: 

```json
"entities": {
    "hashtags": [
    ],
    "urls": [
    ],
    "user_mentions": [
    ],
    "symbols": [
    ]
  }
```

Note that the ```media``` and ```polls``` entities will only appear when that type of content is part of the Tweet.  

## Twitter Extended Entities <a id="extended-entities" class="tall">&nbsp;</a>

If a Tweet contains native media (shared with the Tweet user-interface as opposed via a link to elsewhere), there will also be a ```extended_entities``` section. When it comes to any native media (photo, video, or GIF), the ```extended_entities``` is the preferred metadata source for several reasons. Currently, up to four photos can be attached to a Tweet. The  ```entities``` metadata will only contain the first photo (until 2014, only one photo could be included), while the ```extended_entities``` section will include all attached photos. With native media, another deficiency of the ```entities.media``` metadata is that the media type will always indicate 'photo', even in cases where the attached media is a video or animated GIF. The actual type of media is specified in the ```extended_entities.media[].type``` attribute and is set to either _photo_, _video_, or _animated_gif_. For these reasons, if you are working with native media, the ```extended_entities``` metadata is the way to go. 

### Parsing Tips

Consumers of ```entities``` and ```entities_extended``` sections must be tolerant of 'missing' fields, since not all fields appear in all contexts. Parsers should tolerate the addition of new fields and variance in ordering of fields with ease. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing.

[OTHER TIPS? MOVE TO GLOBAL introductory section]

The ```entities``` and ```extended_entities``` sections are both made up of arrays of entity _objects_. Below you will find descriptions for each of these entity objects, including data dictionaries that describe the object attribute names, types, and short description. We'll also indicate which PowerTrack Operators match these attributes, and include some sample JSON payloads. 

## Entities Data Dictionary <a id="entities-data-dictionary" class="tall">&nbsp;</a>

A collection of common entities found in Tweets, including hashtags, links, and user mentions. This ```entities``` object does include a ```media``` attribute, but its implementation in the ```entiites``` section is only completely accurate for Tweets with a single photo. For all Tweets with more than one photo, a video, or animated GIF, the reader is directed to the ```extended_entities``` section. 

### Entities object

The entities object is a holder of arrays of other entity sub-objects. After illustrating the ```entities``` structure, data dictionaries for these sub-objects, and the Operators that match them, will be provided.

<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead valign="bottom">
<tr class="row-odd"><th class="head">Field</th>
<th class="head">Type</th>
<th class="head">Description</th>
</tr>
</thead>
<tbody valign="top">

<tr class="row-even"><td>hashtags</td>
<td>Array of <a class="reference external" href="#hashtag">Hashtag Objects</a></td>
<td><p class="first">Represents hashtags which have been parsed out of the Tweet text.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#10;  &#34;hashtags&#34;: [&#10;    {&#10;      &#34;indices&#34;: [&#10;        32,&#10;        38&#10;      ],&#10;      &#34;text&#34;: &#34;nodejs&#34;&#10;    }&#10;  ]&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>media</td>
<td>Array of <a class="reference external" href="#obj-media">Media Objects</a></td>
<td><p class="first">Represents media elements uploaded with the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#10;  &#34;media&#34;: [&#10;    {&#10;      &#34;type&#34;: &#34;photo&#34;,&#10;      &#34;sizes&#34;: {&#10;        &#34;thumb&#34;: {&#10;          &#34;h&#34;: 150,&#10;          &#34;resize&#34;: &#34;crop&#34;,&#10;          &#34;w&#34;: 150&#10;        },&#10;        &#34;large&#34;: {&#10;          &#34;h&#34;: 238,&#10;          &#34;resize&#34;: &#34;fit&#34;,&#10;          &#34;w&#34;: 226&#10;        },&#10;        &#34;medium&#34;: {&#10;          &#34;h&#34;: 238,&#10;          &#34;resize&#34;: &#34;fit&#34;,&#10;          &#34;w&#34;: 226&#10;        },&#10;        &#34;small&#34;: {&#10;          &#34;h&#34;: 238,&#10;          &#34;resize&#34;: &#34;fit&#34;,&#10;          &#34;w&#34;: 226&#10;        }&#10;      },&#10;      &#34;indices&#34;: [&#10;        15,&#10;        35&#10;      ],&#10;      &#34;url&#34;: &#34;http:\/\/t.co\/rJC5Pxsu&#34;,&#10;      &#34;media_url&#34;: &#34;http:\/\/p.twimg.com\/AZVLmp-CIAAbkyy.jpg&#34;,&#10;      &#34;display_url&#34;: &#34;pic.twitter.com\/rJC5Pxsu&#34;,&#10;      &#34;id&#34;: 1.1408049304097e+17,&#10;      &#34;id_str&#34;: &#34;114080493040967680&#34;,&#10;      &#34;expanded_url&#34;: &#34;http:\/\/twitter.com\/yunorno\/status\/114080493036773378\/photo\/1&#34;,&#10;      &#34;media_url_https&#34;: &#34;https:\/\/p.twimg.com\/AZVLmp-CIAAbkyy.jpg&#34;&#10;    }&#10;  ]&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>urls</td>
<td>Array of <a class="reference external" href="#url">URL Objects</a></td>
<td><p class="first">Represents URLs included in the text of a Tweet.</p>
<p>Example (without Enhanced URLs enrichment enabled):</p>
<div class="code javascript highlight-python"><div class="highlight"><pre><span></span>{&#10;  &#34;urls&#34;: [&#10;    {&#10;      &#34;indices&#34;: [&#10;        32,&#10;        52&#10;      ],&#10;      &#34;url&#34;: &#34;http:\/\/t.co\/IOwBrTZR&#34;,&#10;      &#34;display_url&#34;: &#34;youtube.com\/watch?v=oHg5SJ\u2026&#34;,&#10;      &#34;expanded_url&#34;: &#34;http:\/\/www.youtube.com\/watch?v=oHg5SJYRHA0&#34;&#10;    }&#10;  ]&#10;}
</pre></div>
</div>
<p>Example (with Enhanced URLs enrichment enabled):</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#34;urls&#34;: [&#10;      {&#10;        &#34;url&#34;: &#34;https:\/\/t.co\/D0n7a53c2l&#34;,&#10;        &#34;expanded_url&#34;: &#34;http:\/\/bit.ly\/18gECvy&#34;,&#10;        &#34;display_url&#34;: &#34;bit.ly\/18gECvy&#34;,&#10;        &#34;unwound&#34;: {&#10;          &#34;url&#34;: &#34;https:\/\/www.youtube.com\/watch?v=oHg5SJYRHA0&#34;,&#10;          &#34;status&#34;: 200,&#10;          &#34;title&#34;: &#34;RickRoll'D&#34;,&#10;          &#34;description&#34;: &#34;http:\/\/www.facebook.com\/rickroll548 As long as trolls are still trolling, the Rick will never stop rolling.&#34;&#10;        },&#10;        &#34;indices&#34;: [&#10;          62,&#10;          85&#10;        ]&#10;      }&#10;    ]&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>user_mentions</td>
<td>Array of <a class="reference external" href="#user-mention">User Mention Objects</a></td>
<td><p class="first">Represents other Twitter users mentioned in the text of the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#10;  &#34;user_mentions&#34;: [&#10;    {&#10;      &#34;name&#34;: &#34;Twitter API&#34;,&#10;      &#34;indices&#34;: [&#10;        4,&#10;        15&#10;      ],&#10;      &#34;screen_name&#34;: &#34;twitterapi&#34;,&#10;      &#34;id&#34;: 6253282,&#10;      &#34;id_str&#34;: &#34;6253282&#34;&#10;    }&#10;  ]&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>symbols</td>
<td>Array of <a class="reference external" href="#symbol">Symbol Objects</a></td>
<td><p class="first">Represents symbols, i.e. $cashtags, included in the text of the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#10;  &#34;symbols&#34;: [&#10;    {&#10;      &#34;indices&#34;: [&#10;        12,&#10;        17&#10;      ],&#10;      &#34;text&#34;: &#34;twtr&#34;&#10;    }&#10;  ]&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>polls</td>
<td>Array of <a class="reference external" href="#poll">Poll Objects</a></td>
<td><p class="first">Represents Twitter Polls included in the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#34;polls&#34;: [&#10;      {&#10;        &#34;options&#34;: [&#10;          {&#10;            &#34;position&#34;: 1,&#10;            &#34;text&#34;: &#34;I read documentation once.&#34;&#10;          },&#10;          {&#10;            &#34;position&#34;: 2,&#10;            &#34;text&#34;: &#34;I read documentation twice.&#34;&#10;          },&#10;          {&#10;            &#34;position&#34;: 3,&#10;            &#34;text&#34;: &#34;I read documentation over and over again.&#34;&#10;          }&#10;        ],&#10;        &#34;end_datetime&#34;: &#34;Thu May 25 22:20:27 +0000 2017&#34;,&#10;        &#34;duration_minutes&#34;: 60&#10;      }&#10;    ]&#10;  }
</pre></div>
</div>
</td>
</tr>

</tbody>
</table>

### Hashtag Object<a id="hashtag" class="tall">&nbsp;</a>

The ```entities``` section will contain a ```hashtags``` array containing an object for every hashtag included in the Tweet body, and include an empty array if no hashtags are present. 

The PowerTrack ```#``` Operator is used to match on the ```text``` attribute. The ```has:hashtags``` Operator will match if there is at least one item in the array. 

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>indices</td>
<td>Array of Int</td>
<td><p class="first">An array of integers indicating the offsets within the Tweet text where the hashtag begins and ends. The first integer represents the
location of the # character in the Tweet text string. The second integer represents the location of the first character after the
hashtag. Therefore the difference between the two numbers will be the length of the hashtag name plus one (for the &#8216;#&#8217; character).
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[32,38]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>text</td>
<td>String</td>
<td><p class="first">Name of the hashtag, minus the leading &#8216;#&#8217; character.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;text&quot;:&quot;nodejs&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

### Media Object <a id="media" class="tall">&nbsp;</a>

The ```entities``` section will contain a ```media``` array containing a single media object if any media object has been 'attached' to the Tweet. If no native media has been attached, there will be no ```media``` array in the ```entities```. For the following reasons the ```extended_entities``` section should be used to process Tweet native media:
+ Media ```type``` will always indicate 'photo' even in cases of a video and GIF being attached to Tweet.
+ Even though up to four photos can be attached, only the first one will be listed in the ```entities``` section.

The ```has:media``` Operator will match if this array is populated. 

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>display_url</td>
<td>String</td>
<td><p class="first">URL of the media to display to clients.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;display_url&quot;:&quot;pic.twitter.com\/rJC5Pxsu&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>expanded_url</td>
<td>String</td>
<td><p class="first">An expanded version of display_url. Links to the media display page.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;expanded_url&quot;: &quot;http:\/\/twitter.com\/yunorno\/status\/114080493036773378\/photo\/1&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>id</td>
<td>Int64</td>
<td><p class="first">ID of the media expressed as a 64-bit integer.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id&quot;:114080493040967680
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>id_str</td>
<td>String</td>
<td><p class="first">ID of the media expressed as a string.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id_str&quot;:&quot;114080493040967680&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>indices</td>
<td>Array of Int</td>
<td><p class="first">An array of integers indicating the offsets within the Tweet text where the URL begins and ends. The first integer represents the
location of the first character of the URL in the Tweet text. The second integer represents the location of the first non-URL
character occurring after the URL (or the end of the string if the URL is the last part of the Tweet text).
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[15,35]
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>media_url</td>
<td>String</td>
<td><p class="first">An <a class="reference external" href="http://">http://</a> URL pointing directly to the uploaded media file.
Example:</p>
<div class="code javascript highlight-python"><div class="highlight"><pre><span></span>&quot;media_url&quot;:&quot;http:\/\/p.twimg.com\/AZVLmp-CIAAbkyy.jpg&quot;
</pre></div>
</div>
<p class="last">For media in direct messages,       <code class="docutils literal"><span class="pre">media_url</span></code>      is the same https URL as       <code class="docutils literal"><span class="pre">media_url_https</span></code>      and must be accessed
via an authenticated twitter.com session or by signing a request with the user&#8217;s access token using OAuth 1.0A. It is not possible to
directly embed these images in a web page.</p>
</td>
</tr>

<tr class="row-even"><td>media_url_https</td>
<td>String</td>
<td><p class="first">An <a class="reference external" href="https://">https://</a> URL pointing directly to the uploaded media file, for embedding on https pages.
Example:</p>
<div class="code javascript highlight-python"><div class="highlight"><pre><span></span>&quot;media_url_https&quot;:&quot;https:\/\/p.twimg.com\/AZVLmp-CIAAbkyy.jpg&quot;
</pre></div>
</div>
<p class="last">For media in direct messages,       <code class="docutils literal"><span class="pre">media_url_https</span></code>      must be accessed via an authenticated twitter.com session or by signing
a request with the user&#8217;s access token using OAuth 1.0A. It is not possible to directly embed these images in a web page.</p>
</td>
</tr>

<tr class="row-odd"><td>sizes</td>
<td><a class="reference external" href="#sizes">Size Object</a></td>
<td><p class="first">An object showing available sizes for the media file.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#10;  &#34;sizes&#34;: {&#10;    &#34;thumb&#34;: {&#10;      &#34;h&#34;: 150,&#10;      &#34;resize&#34;: &#34;crop&#34;,&#10;      &#34;w&#34;: 150&#10;    },&#10;    &#34;large&#34;: {&#10;      &#34;h&#34;: 238,&#10;      &#34;resize&#34;: &#34;fit&#34;,&#10;      &#34;w&#34;: 226&#10;    },&#10;    &#34;medium&#34;: {&#10;      &#34;h&#34;: 238,&#10;      &#34;resize&#34;: &#34;fit&#34;,&#10;      &#34;w&#34;: 226&#10;    },&#10;    &#34;small&#34;: {&#10;      &#34;h&#34;: 238,&#10;      &#34;resize&#34;: &#34;fit&#34;,&#10;      &#34;w&#34;: 226&#10;    }&#10;  }&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>source_status_id</td>
<td>Int64</td>
<td><p class="first">Nullable. For Tweets containing media that was originally associated with a different tweet, this ID points to the original Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;source_status_id&quot;: 205282515685081088
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>source_status_id_str</td>
<td>Int64</td>
<td><p class="first">Nullable. For Tweets containing media that was originally associated with a different tweet, this string-based ID points to the original Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;source_status_id_str&quot;: &quot;205282515685081088&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>type</td>
<td>String</td>
<td><p class="first">Type of uploaded media. Possible types include photo, video, and animated_gif.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;type&quot;:&quot;photo&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>url</td>
<td>String</td>
<td><p class="first">Wrapped URL for the media link. This corresponds with the URL embedded directly into the raw Tweet text, and the values for the
<code class="docutils literal"><span class="pre">indices</span></code> parameter.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;url&quot;:&quot;http:\/\/t.co\/rJC5Pxsu&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>


### URL Object <a id="url" class="tall">&nbsp;</a>

The ```entities``` section will contain a ```urls``` array containing an object for every link included in the Tweet body, and include an empty array if no links are present. 

The ```has:links``` Operator will match if there is at least one item in the array. The ```url:``` Operator is used to match on the ```expanded_url``` attribute. If you are using the [Expanded URL enrichment](http://support.gnip.com/enrichments/expanded_urls.html), the ```url:``` Operator is used to match on the ```unwound.url``` (fully unwound URL) attribute. If you are using the [Exhanced URL enrichment](http://support.gnip.com/enrichments/enhanced_urls.html), the ```url_title:``` and ```url_decription:``` Operators are used to match on the ```unwound.title``` and ```unwound.description``` attributes.        

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>display_url</td>
<td>String</td>
<td><p class="first">URL pasted/typed into Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;display_url&quot;:&quot;bit.ly\/2so49n2&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>expanded_url</td>
<td>String</td>
<td><p class="first">Expanded version of ``     display_url``     .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;expanded_url&quot;:&quot;http:\/\/bit.ly\/2so49n2&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>indices</td>
<td>Array of Int</td>
<td><p class="first">An array of integers representing offsets within the Tweet text where the URL begins and ends. The first integer represents the
location of the first character of the URL in the Tweet text. The second integer represents the location of the first non-URL
character after the end of the URL.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[30,53]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>url</td>
<td>String</td>
<td><p class="first">Wrapped URL, corresponding to the value embedded directly into the raw Tweet text, and the values for the indices  parameter. Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;url&quot;:&quot;https:\/\/t.co\/yzocNFvJuL&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

If you are using the Expanded and/or Enhanced URL enrichments, the following metadata is available under the ```unwound``` attribute:

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>url</td>
<td>String</td>
<td><p class="first">The fully unwound version of the link included in the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;url&quot;:&quot;https:\/\/blog.twitter.com\/en_us\/topics\/insights\/2016\/using-twitter-as-a-go-to-communication-channel-during-severe-weather-events.html&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>status</td>
<td>Int</td>
<td><p class="first">Final HTTP status of the unwinding process, a '200' indicating success.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>200
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>title</td>
<td>String</td>
<td><p class="first">HTML title for the link.  Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;title&quot;:&quot;Using Twitter as a \u2018go-to\u2019 communication channel during severe weather&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>description</td>
<td>String</td>
<td><p class="first">HTML description for the link.  Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;description&quot;:&quot;Using Twitter as a \u2018go-to\u2019 communication channel during severe weather&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

### User Mention Object <a id="user-mention" class="tall">&nbsp;</a>

The ```entities``` section will contain a ```user_mentions``` array containing an object for every user mention included in the Tweet body, and include an empty array if no user mention is present. 

The PowerTrack ```@``` Operator is used to match on the ```screen_name``` attribute. The ```has:mentions``` Operator will match if there is at least one item in the array. 

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>id</td>
<td>Int64</td>
<td><p class="first">ID of the mentioned user, as an integer.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id&quot;:6253282
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>id_str</td>
<td>String</td>
<td><p class="first">If of the mentioned user, as a string.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id_str&quot;:&quot;6253282&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>indices</td>
<td>Array of Int</td>
<td><p class="first">An array of integers representing the offsets within the Tweet text where the user reference begins and ends. The first integer
represents the location of the &#8216;&#64;&#8217; character of the user mention. The second integer represents the location of the first
non-screenname character following the user mention.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[4,15]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>name</td>
<td>String</td>
<td><p class="first">Display name of the referenced user.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;name&quot;:&quot;Twitter API&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>screen_name</td>
<td>String</td>
<td><p class="first">Screen name of the referenced user.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;screen_name&quot;:&quot;twitterapi&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

### Symbol Object <a id="symbol" class="tall">&nbsp;</a>

The ```entities``` section will contain a ```symbols``` array containing an object for every $cashtag included in the Tweet body, and include an empty array if no symbol is present. 

The PowerTrack ```$``` Operator is used to match on the ```text``` attribute. The ```has:symbols``` Operator will match if there is at least one item in the array. 

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>indices</td>
<td>Array of Int</td>
<td><p class="first">An array of integers indicating the offsets within the Tweet text where the symbol/cashtag begins and ends. The first integer represents the
location of the $ character in the Tweet text string. The second integer represents the location of the first character after the
cashtag. Therefore the difference between the two numbers will be the length of the hashtag name plus one (for the &#8216;$&#8217; character).
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[12,17]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>text</td>
<td>String</td>
<td><p class="first">Name of the cashhtag, minus the leading &#8216;$&#8217; character.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;text&quot;:&quot;twtr&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

### Poll Object <a id="poll" class="tall">&nbsp;</a>

The ```entities``` section will contain a ```polls``` array containing a single ```poll``` object if the Tweet contains a poll.  If no poll is included, there will be no ```polls``` array in the ```entities``` section. 

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>options</td>
<td>Array of Option Object</td>
<td><p class="first">An array of options, each having a poll position, and the text for that position. Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#34;options&#34;: [&#10;          {&#10;            &#34;position&#34;: 1,&#10;            &#34;text&#34;: &#34;I read documentation once.&#34;&#10;          }&#10;      ]&#10;}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>end_datetime</td>
<td>String</td>
<td><p class="first">Time stamp (UTC) of when poll ends.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&#34;end_datetime&#34;: &#34;Thu May 25 22:20:27 +0000 2017&#34;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>duration_minutes</td>
<td>String</td>
<td><p class="first">Duration of poll in minutes.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&#34;duration_minutes&#34;: 60
</pre></div>
</div>
</td>
</tr>

</tbody>
</table>
</div>

### Media Size Objects

All Tweets with native media (photos, video, and GIFs) will include a set of 'thumb', 'small', 'medium', and 'large' sizes with height and width pixel sizes.

#### Sizes Object <a id="sizes" class="tall">&nbsp;</a>
<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>thumb</td>
<td><a class="reference external" href="#obj-size">Size Object</a></td>
<td><p class="first">Information for a thumbnail-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;thumb&quot;:{&quot;h&quot;:150, &quot;resize&quot;:&quot;crop&quot;, &quot;w&quot;:150}
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>large</td>
<td><a class="reference external" href="#obj-size">Size Object</a></td>
<td><p class="first">Information for a large-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;large&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>medium</td>
<td><a class="reference external" href="#obj-size">Size Object</a></td>
<td><p class="first">Information for a medium-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;medium&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>small</td>
<td><a class="reference external" href="#obj-size">Size Object</a></td>
<td><p class="first">Information for a small-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;small&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

#### Size Object <a id="size" class="tall">&nbsp;</a>

<div>
<table border="1" class="docutils">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>

<tr class="row-even"><td>w</td>
<td>Int</td>
<td><p class="first">Width in pixels of this size.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;w&quot;:150
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>h</td>
<td>Int</td>
<td><p class="first">Height in pixels of this size.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;h&quot;:150
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>resize</td>
<td>String</td>
<td><p class="first">Resizing method used to obtain this size. A value of <code class="docutils literal"><span class="pre">fit``means</span> <span class="pre">that</span> <span class="pre">the</span> <span class="pre">media</span> <span class="pre">was</span> <span class="pre">resized</span> <span class="pre">to</span> <span class="pre">fit</span> <span class="pre">one</span> <span class="pre">dimension,</span> <span class="pre">keeping</span>
<span class="pre">its</span> <span class="pre">native</span> <span class="pre">aspect</span> <span class="pre">ratio.</span> <span class="pre">A</span> <span class="pre">value</span> <span class="pre">of</span> <span class="pre">``crop</span></code> means that the media was cropped in order to fit a specific resolution.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;resize&quot;:&quot;crop&quot;
</pre></div>
</div>
</td>
</tr>

</tbody>
</table>
</div>


## Extended Entities Data Dictionary <a id="extended-entities-data-dictionary" class="tall">&nbsp;</a>

All Tweets with attached photos, videos and animated GIFs will include an ```extended_entities``` JSON object. The ```extended_entities``` object contains a single ```media``` array of ```media``` objects (see the ```entities``` section for its data dictionary). No other entity types, such as hashtags and links, are included in the ```extended_entities``` section. The ```media``` object in the ```extended_entities``` section is identical in structure to the one included in the ```entities``` section. 

Tweets can only have one type of media attached to it. For photos, up to four photos can be attached. For videos and GIFs, one can be attached. Since the media ```type``` metadata in the ```extended_entities``` section correctly indicates the media type ('photo', 'video' or 'animated_gif'), and supports up to 4 photos, it is the preferred metadata source for native media.  

```json
{
  "extended_entities": {
    "media": [
      
    ]
  }
}
```

## Native JSON Examples <a id="example-json" class="tall">&nbsp;</a>

Below are some example Tweets and their associated entities metadata.

### Tweet with four native photos

Tweet with hashtag, user mention, cashtag, URL, and four native photos: 

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Test Tweet with <a href="https://twitter.com/MentionThis">@mentionThis</a> <a href="https://twitter.com/search?q=%24twtr&amp;src=ctag">$twtr</a> <a href="https://t.co/RzmrQ6wAzD">https://t.co/RzmrQ6wAzD</a> <a href="https://twitter.com/hashtag/hashtag?src=hash">#hashtag</a> <a href="https://t.co/9r69akA484">pic.twitter.com/9r69akA484</a></p>&mdash; @FloodSocial (@FloodSocial) <a href="https://twitter.com/FloodSocial/status/861627479294746624">May 8, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Here is the ```entities``` section for this Tweet:

```json
{
  "entities": {
    "hashtags": [
      {
        "text": "hashtag",
        "indices": [
          59,
          67
        ]
      }
    ],
    "urls": [
      {
        "url": "https:\/\/t.co\/RzmrQ6wAzD",
        "expanded_url": "http:\/\/bit.ly\/2pUk4be",
        "display_url": "bit.ly\/2pUk4be",
        "unwound": {
          "url": "https:\/\/blog.gnip.com\/tweeting-in-the-rain\/",
          "status": 200,
          "title": "Tweeting in the Rain, Part 1 - Gnip Blog - Social Data and Data Science Blog",
          "description": "If you would have told me a few years ago that one day I\u2019d be comparing precipitation and social media time-series data, I would have assumed you were joking. \u00a0For 13 years at OneRain I helped develop software and monitoring \u2026 Continue reading \u2192"
        },
        "indices": [
          35,
          58
        ]
      }
    ],
    "user_mentions": [
      {
        "screen_name": "MentionThis",
        "name": "Just Me",
        "id": 50247739,
        "id_str": "50247739",
        "indices": [
          16,
          28
        ]
      }
    ],
    "symbols": [
      {
        "text": "twtr",
        "indices": [
          29,
          34
        ]
      }
    ],
    "media": [
      {
        "id": 8.6162747224416e+17,
        "id_str": "861627472244162561",
        "indices": [
          68,
          91
        ],
        "media_url": "http:\/\/pbs.twimg.com\/media\/C_UdnvPUwAE3Dnn.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/media\/C_UdnvPUwAE3Dnn.jpg",
        "url": "https:\/\/t.co\/9r69akA484",
        "display_url": "pic.twitter.com\/9r69akA484",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/861627479294746624\/photo\/1",
        "type": "photo",
        "sizes": {
          "medium": {
            "w": 1200,
            "h": 900,
            "resize": "fit"
          },
          "small": {
            "w": 680,
            "h": 510,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "large": {
            "w": 2048,
            "h": 1536,
            "resize": "fit"
          }
        }
      }
    ]
  }
}
```

Only in this 'extended' payload below will you find the four (maximum) native photos. Notice that the first photo in the array is the same as the single photo included in the non-extended Twitter _entities_ section. The _media_ metadata structure for photos is the same for both _entities_ and _extended_entities_ sections. 

Here is the ```extented_entities``` section for this Tweet:

```json
{
"extended_entities": {
    "media": [
      {
        "id": 8.6162747224416e+17,
        "id_str": "861627472244162561",
        "indices": [
          68,
          91
        ],
        "media_url": "http:\/\/pbs.twimg.com\/media\/C_UdnvPUwAE3Dnn.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/media\/C_UdnvPUwAE3Dnn.jpg",
        "url": "https:\/\/t.co\/9r69akA484",
        "display_url": "pic.twitter.com\/9r69akA484",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/861627479294746624\/photo\/1",
        "type": "photo",
        "sizes": {
          "medium": {
            "w": 1200,
            "h": 900,
            "resize": "fit"
          },
          "small": {
            "w": 680,
            "h": 510,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "large": {
            "w": 2048,
            "h": 1536,
            "resize": "fit"
          }
        }
      },
      {
        "id": 8.616274722442e+17,
        "id_str": "861627472244203520",
        "indices": [
          68,
          91
        ],
        "media_url": "http:\/\/pbs.twimg.com\/media\/C_UdnvPVYAAZbEs.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/media\/C_UdnvPVYAAZbEs.jpg",
        "url": "https:\/\/t.co\/9r69akA484",
        "display_url": "pic.twitter.com\/9r69akA484",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/861627479294746624\/photo\/1",
        "type": "photo",
        "sizes": {
          "small": {
            "w": 680,
            "h": 680,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "medium": {
            "w": 1200,
            "h": 1200,
            "resize": "fit"
          },
          "large": {
            "w": 2048,
            "h": 2048,
            "resize": "fit"
          }
        }
      },
      {
        "id": 8.6162747414415e+17,
        "id_str": "861627474144149504",
        "indices": [
          68,
          91
        ],
        "media_url": "http:\/\/pbs.twimg.com\/media\/C_Udn2UUQAADZIS.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/media\/C_Udn2UUQAADZIS.jpg",
        "url": "https:\/\/t.co\/9r69akA484",
        "display_url": "pic.twitter.com\/9r69akA484",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/861627479294746624\/photo\/1",
        "type": "photo",
        "sizes": {
          "medium": {
            "w": 1200,
            "h": 900,
            "resize": "fit"
          },
          "small": {
            "w": 680,
            "h": 510,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "large": {
            "w": 2048,
            "h": 1536,
            "resize": "fit"
          }
        }
      },
      {
        "id": 8.6162747476071e+17,
        "id_str": "861627474760708096",
        "indices": [
          68,
          91
        ],
        "media_url": "http:\/\/pbs.twimg.com\/media\/C_Udn4nUMAAgcIa.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/media\/C_Udn4nUMAAgcIa.jpg",
        "url": "https:\/\/t.co\/9r69akA484",
        "display_url": "pic.twitter.com\/9r69akA484",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/861627479294746624\/photo\/1",
        "type": "photo",
        "sizes": {
          "small": {
            "w": 680,
            "h": 680,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "medium": {
            "w": 1200,
            "h": 1200,
            "resize": "fit"
          },
          "large": {
            "w": 2048,
            "h": 2048,
            "resize": "fit"
          }
        }
      }
    ]
  }
}
```

### Tweet with native video

Below is the extended entities metadata for this Tweet with a video: https://twitter.com/FloodSocial/status/869318041078820864

```json
{
  "extended_entities": {
    "media": [
      {
        "id": 8.6931798030742e+17,
        "id_str": "869317980307415040",
        "indices": [
          31,
          54
        ],
        "media_url": "http:\/\/pbs.twimg.com\/ext_tw_video_thumb\/869317980307415040\/pu\/img\/t_E6wyADk_PvxuzF.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/ext_tw_video_thumb\/869317980307415040\/pu\/img\/t_E6wyADk_PvxuzF.jpg",
        "url": "https:\/\/t.co\/TLSTTOvvmP",
        "display_url": "pic.twitter.com\/TLSTTOvvmP",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/869318041078820864\/video\/1",
        "type": "video",
        "sizes": {
          "small": {
            "w": 340,
            "h": 604,
            "resize": "fit"
          },
          "large": {
            "w": 720,
            "h": 1280,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "medium": {
            "w": 600,
            "h": 1067,
            "resize": "fit"
          }
        },
        "video_info": {
          "aspect_ratio": [
            9,
            16
          ],
          "duration_millis": 10704,
          "variants": [
            {
              "bitrate": 320000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/869317980307415040\/pu\/vid\/180x320\/FMei8yCw7yc_Z7e-.mp4"
            },
            {
              "bitrate": 2176000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/869317980307415040\/pu\/vid\/720x1280\/octt5pFbISkef8RB.mp4"
            },
            {
              "bitrate": 832000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/869317980307415040\/pu\/vid\/360x640\/2OmqK74SQ9jNX8mZ.mp4"
            },
            {
              "content_type": "application\/x-mpegURL",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/869317980307415040\/pu\/pl\/wcJQJ2nxiFU4ZZng.m3u8"
            }
          ]
        }
      }
    ]
  }
}
```

As discussed above, here is the ```entities``` section that incorrectly has the ```type``` set to 'photo'. Again, the ```extended_entities``` section is preferred for all native media types, including 'video' and 'animated_gif'.

```json
{
"entities": {
    "hashtags": [
      
    ],
    "urls": [
      
    ],
    "user_mentions": [
      
    ],
    "symbols": [
      
    ],
    "media": [
      {
        "id": 8.6931798030742e+17,
        "id_str": "869317980307415040",
        "indices": [
          31,
          54
        ],
        "media_url": "http:\/\/pbs.twimg.com\/ext_tw_video_thumb\/869317980307415040\/pu\/img\/t_E6wyADk_PvxuzF.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/ext_tw_video_thumb\/869317980307415040\/pu\/img\/t_E6wyADk_PvxuzF.jpg",
        "url": "https:\/\/t.co\/TLSTTOvvmP",
        "display_url": "pic.twitter.com\/TLSTTOvvmP",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/869318041078820864\/video\/1",
        "type": "photo",
        "sizes": {
          "small": {
            "w": 340,
            "h": 604,
            "resize": "fit"
          },
          "large": {
            "w": 720,
            "h": 1280,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "medium": {
            "w": 600,
            "h": 1067,
            "resize": "fit"
          }
        }
      }
    ]
  }

}
```


#### Tweet with an animated GIF

Below is the extended entities metadata for this Tweet with an animated GIF: https://twitter.com/FloodSocial/status/870042717589340160

```json
{
  "extended_entities": {
    "media": [
      {
        "id": 8.7004265421346e+17,
        "id_str": "870042654213459968",
        "indices": [
          29,
          52
        ],
        "media_url": "http:\/\/pbs.twimg.com\/tweet_video_thumb\/DBMDLy_U0AAqUWP.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/tweet_video_thumb\/DBMDLy_U0AAqUWP.jpg",
        "url": "https:\/\/t.co\/nD6G4bWSKb",
        "display_url": "pic.twitter.com\/nD6G4bWSKb",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/870042717589340160\/photo\/1",
        "type": "animated_gif",
        "sizes": {
          "medium": {
            "w": 350,
            "h": 262,
            "resize": "fit"
          },
          "small": {
            "w": 340,
            "h": 255,
            "resize": "fit"
          },
          "thumb": {
            "w": 150,
            "h": 150,
            "resize": "crop"
          },
          "large": {
            "w": 350,
            "h": 262,
            "resize": "fit"
          }
        },
        "video_info": {
          "aspect_ratio": [
            175,
            131
          ],
          "variants": [
            {
              "bitrate": 0,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/tweet_video\/DBMDLy_U0AAqUWP.mp4"
            }
          ]
        }
      }
    ]
  }
}
```
