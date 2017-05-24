
<tr class="row-odd"><td>contributors</td>
<td>Collection of Contributors</td>
<td><p class="first"><strong>Deprecated</strong> <em>Nullable</em> A collection of brief user objects (usually only one) indicating users who contributed to the
authorship of the tweet, on behalf of the official tweet author. This is a legacy value and is not actively used.</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;contributors&quot;:
[
    {
        &quot;id&quot;:819797,
        &quot;id_str&quot;:&quot;819797&quot;,
        &quot;screen_name&quot;:&quot;episod&quot;
    }
]
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>scopes</td>
<td>Object</td>
<td><p class="first">A set of key-value pairs indicating the intended contextual delivery of the containing Tweet. Currently used by Twitter&#8217;s Promoted
Products.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;scopes&quot;:{&quot;followers&quot;:false}
</pre></div>
</div>
</td>
</tr>



<tr class="row-even"><td>withheld_copyright</td>
<td>Boolean</td>
<td><p class="first">When present and set to &#8220;true&#8221;, it indicates that this piece of content has been withheld due to a <a class="reference external" href="http://en.wikipedia.org/wiki/Digital_Millennium_Copyright_Act">DMCA
complaint</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;withheld_copyright&quot;: true
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>withheld_in_countries</td>
<td>Array of String</td>
<td><p class="first">When present, indicates a list of uppercase <a class="reference external" href="http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">two-letter country codes</a> this
content is withheld from. Twitter supports the following non-country values for this field:</p>
<p>&#8220;XX&#8221; - Content is withheld in all countries
&#8220;XY&#8221; - Content is withheld due to a DMCA request.</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;withheld_in_countries&quot;: [&quot;GR&quot;, &quot;HK&quot;, &quot;MY&quot;]
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>withheld_scope</td>
<td>String</td>
<td><p class="first">When present, indicates whether the content being withheld is the &#8220;status&#8221; or a &#8220;user.&#8221;</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;withheld_scope&quot;: &quot;status&quot;
</pre></div>
</div>
</td>
</tr>

</tbody>
</table>


### Other attributes served by other sources:

[] Include? Remove?

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

<tr class="row-odd"><td>current_user_retweet</td>
<td>Object</td>
<td><p class="first"><em>Perspectival</em> Only surfaces on methods supporting the      <code class="docutils literal"><span class="pre">include_my_retweet</span></code>     parameter, when set to true. Details the
Tweet ID of the user&#8217;s own retweet (if existent) of this Tweet.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;current_user_retweet&quot;: {
  &quot;id&quot;: 26815871309,
  &quot;id_str&quot;: &quot;26815871309&quot;
}
</pre></div>
</div>
</td>
</tr>
