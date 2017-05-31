[] TODOS
+ [X] Add symbols
+ [X] Add polling metadata
+ [] Add Video extended entities details
+ [] JSON pretty print encoding: https://opinionatedgeek.com/Codecs/HtmlEncoder

```json
"video_info": {
          "aspect_ratio": [
            9,
            16
          ],
          "duration_millis": 28243,
          "variants": [
            {
              "bitrate": 832000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/vid\/360x640\/ESCOjk0Mf6qyb2cH.mp4"
            },
            {
              "bitrate": 320000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/vid\/180x320\/nKsu8KSwL2lo2ez7.mp4"
            },
            {
              "content_type": "application\/x-mpegURL",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/pl\/wCsBDhxFS0Nkakfj.m3u8"
            },
            {
              "bitrate": 2176000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/vid\/720x1280\/n-apEhXDY81_75bq.mp4"
            }
          ]
        }
 ```       


+ [Twitter Entities](#entities)
+ [Entities Data Dictionary](#entities-data-dictionary)
+ [Extended Entities Data Dictionary](#extended-entities-data-dictionary)
+ [Example Native JSON](#example-json)

## Twitter Entities and Extended Entities<a id="entities" class="tall">&nbsp;</a>

Entities provide metadata and additional contextual information about content posted on Twitter. These entities include hashtags, user mentions, URLs, symbols (cashtags), Twitter polls, and _native_ media (photos, videos, and GIFs shared via the Twitter user-interface). 

If a Tweet contains native media, there will also be a _extended_entities_ section. When it comes to native media, the _extended_entities_ is the preferred metadata source for several reasons. Currently, up to four photos can be attached to a Tweet. The  _entities_ metadata will only contain the first photo (until 2014, only one photo could be included), while the _extended_entities_ will include all attached photos. Another deficiency with the _entities.media_ metadata is that the media type will always indicate 'photo', even in cases where the attached media is a video or animated GIF. When it comes to native media, the _extended_entities_ metadata is the way to go.   

Entities are never divorced from the content they describe. Entities are returned wherever Tweets are found in the API. Entities are instrumental in resolving URLs. The core 'top-level' entities array structures are present, even when corresponding entities are not present in the Tweet, with one exception. For example, a Tweet with no hashtags will still have the following JSON structure:

```json
{
  "entities": {
    "hashtags": [
      
    ]
  }
}
```
The one exception is poll data. The 'polls' metadata will only be present when the Tweet contains a Twitter poll. 

The _extended_entities_ metadata is present when any 'native' media is attached using the Twitter user-interface. This includes up to four photos, a single GIF, or a single video. The type of media is specified in the _extended_entities.media[].type_ attribute and is set to either _photo_, _video_, or _animated_gif_.

## Entities Data Dictionary <a id="entities-data-dictionary" class="tall">&nbsp;</a>

Consumers of Entities should tolerate the addition of new fields and variance in ordering of fields with ease. Not all fields appear in all contexts. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing.</p>

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
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;hashtags&quot;:[{&quot;indices&quot;:[32,36],&quot;text&quot;:&quot;lol&quot;}]
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>media</td>
<td>Array of <a class="reference external" href="#obj-media">Media Objects</a></td>
<td><p class="first">Represents media elements uploaded with the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;media&quot;:[{&quot;type&quot;:&quot;photo&quot;, &quot;sizes&quot;:{&quot;thumb&quot;:{&quot;h&quot;:150, &quot;resize&quot;:&quot;crop&quot;, &quot;w&quot;:150}, &quot;large&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226},
&quot;medium&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}, &quot;small&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}}, &quot;indices&quot;:[15,35],
&quot;url&quot;:&quot;http:\/\/t.co\/rJC5Pxsu&quot;, &quot;media_url&quot;:&quot;http:\/\/p.twimg.com\/AZVLmp-CIAAbkyy.jpg&quot;,
&quot;display_url&quot;:&quot;pic.twitter.com\/rJC5Pxsu&quot;,&quot;id&quot;:114080493040967680, &quot;id_str&quot;:&quot;114080493040967680&quot;, &quot;expanded_url&quot;:
&quot;http:\/\/twitter.com\/yunorno\/status\/114080493036773378\/photo\/1&quot;,
&quot;media_url_https&quot;:&quot;https:\/\/p.twimg.com\/AZVLmp-CIAAbkyy.jpg&quot;}]
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>urls</td>
<td>Array of <a class="reference external" href="#url">URL Objects</a></td>
<td><p class="first">Represents URLs included in the text of a Tweet or within textual fields of a <a class="reference external" href="/overview/api/users">user object</a> .
Tweet Example:</p>
<div class="code javascript highlight-python"><div class="highlight"><pre><span></span>&quot;urls&quot;:[{&quot;indices&quot;:[32,52], &quot;url&quot;:&quot;http:\/\/t.co\/IOwBrTZR&quot;, &quot;display_url&quot;:&quot;youtube.com\/watch?v=oHg5SJ\u2026&quot;,
&quot;expanded_url&quot;:&quot;http:\/\/www.youtube.com\/watch?v=oHg5SJYRHA0&quot;}]
</pre></div>
</div>
<p>User Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span> &quot;urls&quot;:[{&quot;indices&quot;:[32,52], &quot;url&quot;:&quot;http:\/\/t.co\/IOwBrTZR&quot;, &quot;display_url&quot;:&quot;youtube.com\/watch?v=oHg5SJ\u2026&quot;, &quot;expanded_url&quot;
:&quot;http:\/\/www.youtube.com\/watch?v=oHg5SJYRHA0&quot;}]
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>user_mentions</td>
<td>Array of <a class="reference external" href="#user-mention">User Mention Objects</a></td>
<td><p class="first">Represents other Twitter users mentioned in the text of the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;user_mentions&quot;:[{&quot;name&quot;:&quot;Twitter API&quot;, &quot;indices&quot;:[4,15], &quot;screen_name&quot;:&quot;twitterapi&quot;, &quot;id&quot;:6253282, &quot;id_str&quot;:&quot;6253282&quot;}]
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>symbols</td>
<td>Array of <a class="reference external" href="#symbol">Symbol Objects</a></td>
<td><p class="first">Represents symbols, i.e. $cashtags, included in the text of the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;symbols&quot;:[{&quot;indices&quot;:[12,17],&quot;text&quot;:&quot;twtr&quot;}]
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>polls</td>
<td>Array of <a class="reference external" href="#poll">Poll Objects</a></td>
<td><p class="first">Represents Twitter Pools included in the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#34;polls&#34;: [&#10;      {&#10;        &#34;options&#34;: [&#10;          {&#10;            &#34;position&#34;: 1,&#10;            &#34;text&#34;: &#34;I read documentation once.&#34;&#10;          },&#10;          {&#10;            &#34;position&#34;: 2,&#10;            &#34;text&#34;: &#34;I read documentation twice.&#34;&#10;          },&#10;          {&#10;            &#34;position&#34;: 3,&#10;            &#34;text&#34;: &#34;I read documentation over and over again.&#34;&#10;          }&#10;        ],&#10;        &#34;end_datetime&#34;: &#34;Thu May 25 22:20:27 +0000 2017&#34;,&#10;        &#34;duration_minutes&#34;: 60&#10;      }&#10;    ]&#10;  }
</pre></div>
</div>
</td>
</tr>

</tbody>
</table>

### Hashtag <a id="hashtag" class="tall">&nbsp;</a>

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
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[32,36]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>text</td>
<td>String</td>
<td><p class="first">Name of the hashtag, minus the leading &#8216;#&#8217; character.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;text&quot;:&quot;lol&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

### Media <a id="media" class="tall">&nbsp;</a>

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
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;sizes&quot;:{&quot;thumb&quot;:{&quot;h&quot;:150, &quot;resize&quot;:&quot;crop&quot;, &quot;w&quot;:150}, &quot;large&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}, &quot;medium&quot;:{&quot;h&quot;:238, &quot;resize&quot;:
&quot;fit&quot;, &quot;w&quot;:226}, &quot;small&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}}
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>source_status_id</td>
<td><a class="reference external" href="#obj-sizes">Int64</a></td>
<td><p class="first">Nullable. For Tweets containing media that was originally associated with a different tweet, this ID points to the original Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;source_status_id&quot;: 205282515685081088
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>source_status_id_str</td>
<td><a class="reference external" href="#obj-sizes">Int64</a></td>
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

### Size <a id="size" class="tall">&nbsp;</a>

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
<tr class="row-even"><td>w</td>
<td>Int</td>
<td><p class="first">Width in pixels of this size.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;w&quot;:150
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

### Sizes <a id="sizes" class="tall">&nbsp;</a>
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
<td><a class="reference external" href="#obj-size">Object</a></td>
<td><p class="first">Information for a thumbnail-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;thumb&quot;:{&quot;h&quot;:150, &quot;resize&quot;:&quot;crop&quot;, &quot;w&quot;:150}
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>large</td>
<td><a class="reference external" href="#obj-size">Object</a></td>
<td><p class="first">Information for a large-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;large&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>medium</td>
<td><a class="reference external" href="#obj-size">Object</a></td>
<td><p class="first">Information for a medium-sized version of the media.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;medium&quot;:{&quot;h&quot;:238, &quot;resize&quot;:&quot;fit&quot;, &quot;w&quot;:226}
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>small</td>
<td><a class="reference external" href="#obj-size">Object</a></td>
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

### URL <a id="url" class="tall">&nbsp;</a>
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
<td><p class="first">Version of the URL to display to clients.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;display_url&quot;:&quot;youtube.com\/watch?v=oHg5SJ\u2026&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>expanded_url</td>
<td>String</td>
<td><p class="first">Expanded version of ``     display_url``     .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;expanded_url&quot;:&quot;http:\/\/www.youtube.com\/watch?v=oHg5SJYRHA0&quot;
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
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;indices&quot;:[32,52]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>url</td>
<td>String</td>
<td><p class="first">Wrapped URL, corresponding to the value embedded directly into the raw Tweet text, and the values for the
.. code:: javascript</p>
<blockquote>
<div>indices</div></blockquote>
<p>parameter.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;url&quot;:&quot;http:\/\/t.co\/IOwBrTZR&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>

### User Mention <a id="user-mention" class="tall">&nbsp;</a>
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

### Symbols <a id="symbol" class="tall">&nbsp;</a>
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


### Polls <a id="poll" class="tall">&nbsp;</a>
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
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>{&#34;options&#34;: [&#10;          {&#10;            &#34;position&#34;: 1,&#10;            &#34;text&#34;: &#34;Testing is important&#34;&#10;          }&#10;      ]&#10;}
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



## Extended Entities Data Dictionary <a id="extended-entities-data-dictionary" class="tall">&nbsp;</a>

```json
{
  "extended_entities": {
    "media": [
      
    ]
  }
}
```


Tweet with hashtag, user mention, cashtag, URL, and four native photos: https://twitter.com/FloodSocial/status/861627479294746624
Quoted Tweet of that one containing new text, hashtag, user mention, and cashtag: https://twitter.com/FloodSocial/status/865604154676432896


## Native JSON Examples <a id="example-json" class="tall">&nbsp;</a>


### Twitter _entities_

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


### Twitter _extended_entities_


#### Tweet with four photos

Below is the extended entities metadata for this Tweet with four photos: https://twitter.com/FloodSocial/status/861627479294746624

Only in this 'extended' payload will you find the four (maximum) native photos. Notice that the first photo in the array is the same as the single photo included in the non-extended Twitter _entities_ section. The _media_ metadata structure for photos is the same for both _entities_ and _extended_entities_ sections. 

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

#### Tweet with native video

Below is an example Tweet with a native video:

```json

{
  "extended_entities": {
    "media": [
      {
        "id": 8.6783322948264e+17,
        "id_str": "867833229482635265",
        "indices": [
          11,
          34
        ],
        "media_url": "http:\/\/pbs.twimg.com\/ext_tw_video_thumb\/867833229482635265\/pu\/img\/ugKcw5gwMum1OPbm.jpg",
        "media_url_https": "https:\/\/pbs.twimg.com\/ext_tw_video_thumb\/867833229482635265\/pu\/img\/ugKcw5gwMum1OPbm.jpg",
        "url": "https:\/\/t.co\/7bk3AFRSZu",
        "display_url": "pic.twitter.com\/7bk3AFRSZu",
        "expanded_url": "https:\/\/twitter.com\/FloodSocial\/status\/867833312600932352\/video\/1",
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
          "duration_millis": 28243,
          "variants": [
            {
              "bitrate": 832000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/vid\/360x640\/ESCOjk0Mf6qyb2cH.mp4"
            },
            {
              "bitrate": 320000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/vid\/180x320\/nKsu8KSwL2lo2ez7.mp4"
            },
            {
              "content_type": "application\/x-mpegURL",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/pl\/wCsBDhxFS0Nkakfj.m3u8"
            },
            {
              "bitrate": 2176000,
              "content_type": "video\/mp4",
              "url": "https:\/\/video.twimg.com\/ext_tw_video\/867833229482635265\/pu\/vid\/720x1280\/n-apEhXDY81_75bq.mp4"
            }
          ]
        }
      }
    ]
  }
}
```
