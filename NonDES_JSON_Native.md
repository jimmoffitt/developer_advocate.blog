
## Other attributes served by other sources:

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

## User object details

## Other Attributes Provided with Other Endpoints?

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

<tr class="row-odd"><td>entities</td>
<td><a class="reference external" href="/overview/api/entities">Entities</a></td>
<td><p class="first">Entities which have been parsed out of the <code class="docutils literal"><span class="pre">url</span></code> or <code class="docutils literal"><span class="pre">description</span></code> fields defined by the user. Read more
about <a class="reference external" href="/overview/api/entities">User Entities</a> .</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;entities&quot;: {
  &quot;url&quot;: {
    &quot;urls&quot;: [
      {
        &quot;url&quot;: &quot;http://dev.twitter.com&quot;,
        &quot;expanded_url&quot;: null,
        &quot;indices&quot;: [0, 22]
      }
    ]
  },
  &quot;description&quot;: {&quot;urls&quot;:[] }
}
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>follow_request_sent</td>
<td>Type</td>
<td><p class="first"><em>Nullable</em> . <em>Perspectival</em> . When true, indicates that the authenticating user has issued a follow request to this protected user
account.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;follow_request_sent&quot;: false
</pre></div>
</div>
</td>
</tr>





<tr class="row-even"><td>status</td>
<td><a class="reference external" href="/overview/api/tweets">Tweets</a></td>
<td><p class="first"><em>Nullable</em> . If possible, the user&#8217;s most recent Tweet or retweet. In some circumstances, this data cannot be provided and this field
will be omitted, null, or empty. Perspectival attributes within Tweets embedded within users cannot always be relied upon.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;status&quot;: {
    &quot;coordinates&quot;: null,
    &quot;favorited&quot;: false,
    &quot;truncated&quot;: false,
    &quot;created_at&quot;: &quot;Tue Apr 17 16:38:18 +0000 2012&quot;,
    &quot;id_str&quot;: &quot;192290904646754304&quot;,
    &quot;entities&quot;: {
      &quot;urls&quot;: [

      ],
      &quot;hashtags&quot;: [

      ],
      &quot;user_mentions&quot;: [
        {
          &quot;name&quot;: &quot;Micah McVicker&quot;,
          &quot;id_str&quot;: &quot;166661446&quot;,
          &quot;id&quot;: 166661446,
          &quot;indices&quot;: [
            0,
            14
          ],
          &quot;screen_name&quot;: &quot;MicahMcVicker&quot;
        }
      ]
    },
    &quot;in_reply_to_user_id_str&quot;: &quot;166661446&quot;,
    &quot;contributors&quot;: null,
    &quot;text&quot;: &quot;@MicahMcVicker make sure you&#39;re using include_rts=true and no other filters...&quot;
    &quot;retweet_count&quot;: 0,
    &quot;in_reply_to_status_id_str&quot;: &quot;192290470427246594&quot;,
    &quot;id&quot;: 192290904646754304,
    &quot;geo&quot;: null,
    &quot;retweeted&quot;: false,
    &quot;in_reply_to_user_id&quot;: 166661446,
    &quot;place&quot;: null,
    &quot;in_reply_to_screen_name&quot;: &quot;MicahMcVicker&quot;,
    &quot;source&quot;: &quot;YoruFukurou&quot;,
    &quot;in_reply_to_status_id&quot;: 192290470427246594
 },
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>withheld_in_countries</td>
<td>String</td>
<td><p class="first">When present, indicates a textual representation of the two-letter country codes this user is withheld from.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;withheld_in_countries&quot;: &quot;GR, HK, MY&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>withheld_scope</td>
<td>String</td>
<td><p class="first">When present, indicates whether the content being withheld is the &#8220;status&#8221; or a &#8220;user.&#8221;
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;withheld_scope&quot;: &quot;user&quot;
</pre></div>
</div>
</td>
</tr>



</tbody>
</table>





