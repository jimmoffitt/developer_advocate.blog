[] TODOS
[] Add symbols

+ [Twitter Entities](#entities)
+ [Entities Data Dictionary](#entities-data-dictionary)
+ [Extended Entities Data Dictionary](#extended-entities-data-dictionary)

## Twitter Entities <a id="entities" class="tall">&nbsp;</a>

Entities provide metadata and additional contextual information about content posted on Twitter. Entities are never divorced from the content they describe. Entities are returned wherever Tweets are found in the API. Entities are instrumental in resolving URLs.

< More about entities > 

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
<td>Array of <a class="reference external" href="#obj-hashtags">Object</a></td>
<td><p class="first">Represents hashtags which have been parsed out of the Tweet text.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;hashtags&quot;:[{&quot;indices&quot;:[32,36],&quot;text&quot;:&quot;lol&quot;}]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>media</td>
<td>Array of <a class="reference external" href="#obj-media">Object</a></td>
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
<td>Array of <a class="reference external" href="#obj-url">Object</a></td>
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
<td>Array of <a class="reference external" href="#obj-usermention">Object</a></td>
<td><p class="first">Represents other Twitter users mentioned in the text of the Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;user_mentions&quot;:[{&quot;name&quot;:&quot;Twitter API&quot;, &quot;indices&quot;:[4,15], &quot;screen_name&quot;:&quot;twitterapi&quot;, &quot;id&quot;:6253282, &quot;id_str&quot;:&quot;6253282&quot;}]
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
<div class="section" id="hashtags">
<h3>Hashtags<a class="headerlink" href="#hashtags" title="Permalink to this headline">¶</a></h3>
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
<div class="section" id="media">
<h3>Media<a class="headerlink" href="#media" title="Permalink to this headline">¶</a></h3>
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
<td><a class="reference external" href="#obj-sizes">Object</a></td>
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
<td><p class="first">For Tweets containing media that was originally associated with a different tweet, this ID points to the original Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;source_status_id&quot;: 205282515685081088
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>source_status_id_str</td>
<td><a class="reference external" href="#obj-sizes">Int64</a></td>
<td><p class="first">For Tweets containing media that was originally associated with a different tweet, this string-based ID points to the original Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;source_status_id_str&quot;: &quot;205282515685081088&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>type</td>
<td>String</td>
<td><p class="first">Type of uploaded media.
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
<div class="section" id="size">
<h3>Size<a class="headerlink" href="#size" title="Permalink to this headline">¶</a></h3>
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
<div class="section" id="sizes">
<h3>Sizes<a class="headerlink" href="#sizes" title="Permalink to this headline">¶</a></h3>
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
<div class="section" id="url">
<h3>URL<a class="headerlink" href="#url" title="Permalink to this headline">¶</a></h3>
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
<div class="section" id="user-mention">
<h3>User Mention<a class="headerlink" href="#user-mention" title="Permalink to this headline">¶</a></h3>
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


## Extended Entities Data Dictionary <a id="extended-entities-data-dictionary" class="tall">&nbsp;</a>

