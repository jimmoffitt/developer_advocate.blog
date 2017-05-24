 <div class="section" id="places">
<h1>Places<a class="headerlink" href="#places" title="Permalink to this headline">¶</a></h1>
<div class="toctree-wrapper compound" id="id1">
</div>
<p>Places are specific, named locations with corresponding geo coordinates. They can be attached to <a class="reference external" href="/overview/api/tweets">Tweets</a> by specifying a <code class="docutils literal"><span class="pre">place_id</span></code> when <a class="reference external" href="/rest/reference/post/statuses/update">tweeting</a>. Tweets associated with places are not necessarily issued from that location but could also potentially be <em>about</em> that location.&nbsp;Places can be <a class="reference external" href="/rest/reference/get/geo/search">searched
for</a>. Tweets can also be <a class="reference external" href="/rest/public/finding-tweets-about-places">found</a> by place_id.</p>
<p>Places also have an attributes field that further describes a Place. These attributes are more convention rather than standard practice, and reflect information captured in the Twitter places database.&nbsp;See <a class="reference external" href="#place_attributes">Place Attributes</a> for more information.</p>
<ul class="simple">
<li><a class="reference external" href="#field_guide">Places Field Guide</a></li>
<li><a class="reference external" href="#attributes">Places Attributes</a></li>
</ul>
<div class="section" id="field-guide">
<h2>Field Guide<a class="headerlink" href="#field-guide" title="Permalink to this headline">¶</a></h2>
<p>Consumers of Places should tolerate the addition of new fields and variance in ordering of fields with ease. Not all fields appear in all contexts. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing.</p>
<table border="1" class="docutils">
<colgroup>
<col width="8%" />
<col width="16%" />
<col width="76%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>attributes</td>
<td>Object</td>
<td><p class="first">Contains a hash of variant information about the place. See <a class="reference external" href="#place_attributes">Place Attributes</a> &nbsp;for more detail.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;attributes&quot;: {
    &quot;street_address&quot;: &quot;795 Folsom St&quot;,
    &quot;623:id&quot;: &quot;210176&quot;,
    &quot;twitter&quot;: &quot;twitter&quot;
},
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>bounding_box</td>
<td><a class="reference external" href="#obj-boundingbox">Object</a></td>
<td><p class="first">A bounding box of coordinates which encloses this place.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;bounding_box&quot;:{&quot;coordinates&quot;:[ [ [2.2241006,48.8155414], [2.4699099,48.8155414], [2.4699099,48.9021461], [2.2241006,48.9021461] ] ], &quot;type&quot;:&quot;Polygon&quot;}
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>country</td>
<td>String</td>
<td><p class="first">Name of the country containing this place.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;country&quot;:&quot;France&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>country_code</td>
<td>String</td>
<td><p class="first">Shortened country code representing the country containing this place.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;country_code&quot;:&quot;FR&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>full_name</td>
<td>String</td>
<td><p class="first">Full human-readable representation of the place&#8217;s name.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;full_name&quot;:&quot;San Francisco, CA&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>id</td>
<td>String</td>
<td><p class="first">ID representing this place. Note that this is represented as a string, not an integer.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id&quot;:&quot;7238f93a3e899af6&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>name</td>
<td>String</td>
<td><p class="first">Short human-readable representation of the place&#8217;s name.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;name&quot;:&quot;Paris&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>place_type</td>
<td>String</td>
<td><p class="first">The type of location represented by this place.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;place_type&quot;:&quot;city&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>url</td>
<td>String</td>
<td><p class="first">URL representing the location of additional place metadata for this place.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;url&quot;:&quot;https://api.twitter.com/1.1/geo/id/7238f93a3e899af6.json&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
<div class="section" id="bounding-box">
<h3>Bounding box<a class="headerlink" href="#bounding-box" title="Permalink to this headline">¶</a></h3>
<table border="1" class="docutils">
<colgroup>
<col width="4%" />
<col width="9%" />
<col width="87%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Field</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>coordinates</td>
<td>Array of Array of Array of Float</td>
<td><p class="first">A series of longitude and latitude points, defining a box which will contain the Place entity this bounding box is related to. Each point is an array in the form of [longitude, latitude]. Points are grouped into an array per bounding box. Bounding box arrays are wrapped in one additional array to be compatible with the polygon notation.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;coordinates&quot;:[ [ [2.2241006,48.8155414], [2.4699099,48.8155414], [2.4699099,48.9021461], [2.2241006,48.9021461] ] ]
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>type</td>
<td>String</td>
<td><p class="first">The type of data encoded in the coordinates property. This will be &#8220;Polygon&#8221; for bounding boxes.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;type&quot;:&quot;Polygon&quot;
</pre></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>

<div class="section" id="place-attributes">
<h2>Place Attributes<a class="headerlink" href="#place-attributes" title="Permalink to this headline">¶</a></h2>
<p>Place Attributes are metadata about places. An attribute is a key-value pair of arbitrary strings, but with some conventions.</p>
<p>Below are a number of well-known place attributes which may, or may not exist in the returned data. These attributes are provided when the place was created in the Twitter places database.</p>
<table border="1" class="docutils">
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td>Key</td>
<td>Description</td>
</tr>
<tr class="row-even"><td>street_address</td>
<td>&nbsp;</td>
</tr>
<tr class="row-odd"><td>locality</td>
<td>the city the place is in</td>
</tr>
<tr class="row-even"><td>region</td>
<td>the administrative region the place is in</td>
</tr>
<tr class="row-odd"><td>iso3</td>
<td>the country code</td>
</tr>
<tr class="row-even"><td>postal_code</td>
<td>in the preferred local format for the place</td>
</tr>
<tr class="row-odd"><td>phone</td>
<td>in the preferred local format for the place, include long distance code</td>
</tr>
<tr class="row-even"><td>twitter</td>
<td>twitter screen-name, without &#64;</td>
</tr>
<tr class="row-odd"><td>url</td>
<td>official/canonical URL for place</td>
</tr>
<tr class="row-even"><td>app:id</td>
<td>An ID or comma separated list of IDs representing the place in the applications place database.</td>
</tr>
</tbody>
</table>


## Places JSON Examples

