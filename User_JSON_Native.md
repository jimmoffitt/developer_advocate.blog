[] TODOS
+ [] profile attributes not in order
+ [] Keep Public API only attributes?
+ [] Narratives: ignoring deprecated fields. Any others static/constant/useless? 


# Twitter User Object Data Dictionary


<INTRO>

Consumers of User objects should tolerate the addition of new fields and variance in ordering of fields with ease. Not all fields appear in all contexts. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing.</p>

## User Object

The 'user' object contains public Twitter account metadata and describes the *author* of the Tweet. 

In general the metadata values are relatively constant. 
Some fields never change
Some commonly change
Some frequently change



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

<tr class="row-even"><td>id</td>
<td>Int64</td>
<td><p class="first">The integer representation of the unique identifier for this User. This number is greater than 53 bits and some programming languages
may have difficulty/silent defects in interpreting it. Using a signed 64 bit integer for storing this identifier is safe. Use
<code class="docutils literal"><span class="pre">id_str</span></code> for fetching the identifier to stay on the safe side.
See <a class="reference external" href="/overview/api/twitter-ids-json-and-snowflake">Twitter IDs, JSON and Snowflake</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id&quot;: 6253282
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>id_str</td>
<td>String</td>
<td><p class="first">The string representation of the unique identifier for this User. Implementations should use this rather than the large, possibly
un-consumable integer in <code class="docutils literal"><span class="pre">id</span></code>.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;id_str&quot;: &quot;6253282&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>name</td>
<td>String</td>
<td><p class="first">The name of the user, as they&#8217;ve defined it. Not necessarily a person&#8217;s name. Typically capped at 20 characters, but subject to
change.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;name&quot;: &quot;Twitter API&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>screen_name</td>
<td>String</td>
<td><p class="first">The screen name, handle, or alias that this user identifies themselves with. screen_names are unique but subject to change. Use
<code class="docutils literal"><span class="pre">id_str</span></code> as a user identifier whenever possible. Typically a maximum of 15 characters long, but some historical accounts
may exist with longer names.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;screen_name&quot;: &quot;twitterapi&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>location</td>
<td>String</td>
<td><p class="first"><em>Nullable</em> . The user-defined location for this account&#8217;s profile. Not necessarily a location, nor machine-parseable.
This field will occasionally be fuzzily interpreted by the Search service.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;location&quot;: &quot;San Francisco, CA&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>url</td>
<td>String</td>
<td><p class="first"><em>Nullable</em> . A URL provided by the user in association with their profile.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;url&quot;: &quot;https://dev.twitter.com&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>description</td>
<td>String</td>
<td><p class="first"><em>Nullable</em> . The user-defined UTF-8 string describing their account.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;description&quot;: &quot;The Real Twitter API.&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>derived</td>
<td>Arrays of Enrichment Objects</td>
<td><p class="first">Collection of Enrichment metadata derived for user. Provides the <em>Profile Geo</em> and <em>Klout</em> Enrichment metadata. See more documentation <a class="reference external" href="http://support.gnip.com/enrichments/">HERE</a>.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;derived&quot;: {
		&quot;locations&quot;: [{}],
                &quot;klout&quot;: [{}]
          }
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>protected</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that this user has chosen to protect their Tweets. See <a class="reference external" href="https://support.twitter.com/articles/14016-about-public-and-protected-tweets">About Public and Protected
Tweets</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;protected&quot;: true
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>verified</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that the user has a verified account. See <a class="reference external" href="https://support.twitter.com/articles/119135-faqs-about-verified-accounts">Verified
Accounts</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;verified&quot;: false
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>followers_count</td>
<td>Int</td>
<td><p class="first">The number of followers this account currently has. Under certain conditions of duress, this field will temporarily indicate &#8220;0&#8221;.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;followers_count&quot;: 21
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>friends_count</td>
<td>Int</td>
<td><p class="first">The number of users this account is following (AKA their &#8220;followings&#8221;). Under certain conditions of duress, this field will
temporarily indicate &#8220;0&#8221;.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;friends_count&quot;: 32
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>listed_count</td>
<td>Int</td>
<td><p class="first">The number of public lists that this user is a member of.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;listed_count&quot;: 9274
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>favourites_count</td>
<td>Int</td>
<td><p class="first">The number of Tweets this user has liked in the account&#8217;s lifetime. British spelling used in the field name for historical
reasons.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;favourites_count&quot;: 13
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>statuses_count</td>
<td>Int</td>
<td><p class="first">The number of Tweets (including retweets) issued by the user.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;statuses_count&quot;: 42
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>created_at</td>
<td>String</td>
<td><p class="first">The UTC datetime that the user account was created on Twitter.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;created_at&quot;: &quot;Mon Nov 29 21:18:15 +0000 2010&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>utc_offset</td>
<td>Int</td>
<td><p class="first"><em>Nullable</em> . The offset from GMT/UTC in seconds.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;utc_offset&quot;: -18000
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>time_zone</td>
<td>String</td>
<td><p class="first"><em>Nullable</em> . A string describing the Time Zone this user declares themselves within.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;time_zone&quot;: &quot;Pacific Time (US &amp; Canada)&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>geo_enabled</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that the user has enabled the possibility of geotagging their Tweets. This field must be true for the current
user to attach geographic data when using <a class="reference external" href="/rest/reference/post/statuses/update">POST statuses / update</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;geo_enabled&quot;: true
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>lang</td>
<td>String</td>
<td><p class="first">The <a class="reference external" href="http://tools.ietf.org/html/bcp47">BCP 47</a> code for the user&#8217;s self-declared user interface language. May or may not have
anything to do with the content of their Tweets.
Examples:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;lang&quot;: &quot;en&quot;
&quot;lang&quot;: &quot;msa&quot;
&quot;lang&quot;: &quot;zh-cn&quot;
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>contributors_enabled</td>
<td>Boolean</td>
<td><p class="first">Indicates that the user has an account with &#8220;contributor mode&#8221; enabled, allowing for Tweets issued by the user to be co-authored by
another account. Rarely <code class="docutils literal"><span class="pre">true</span></code> (this is a legacy field)
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;contributors_enabled&quot;: false
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>profile_background_color</td>
<td>String</td>
<td><p class="first">The hexadecimal color chosen by the user for their background.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_background_color&quot;: &quot;e8f2f7&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td><dl class="first last docutils">
<dt>profile_background</dt>
<dd>_image_url</dd>
</dl>
</td>
<td>String</td>
<td><p class="first">A HTTP-based URL pointing to the background image the user has uploaded for their profile.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_background_image_url&quot;:
&quot;http://a2.twimg.com/profile_background_images/229557229/twitterapi-bg.png&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td><dl class="first last docutils">
<dt>profile_background_</dt>
<dd>image_url_https</dd>
</dl>
</td>
<td>String</td>
<td><p class="first">A HTTPS-based URL pointing to the background image the user has uploaded for their profile.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_background_image_url_https&quot;:
&quot;https://si0.twimg.com/profile_background_images/229557229/twitterapi-bg.png&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>profile_background_tile</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that the user&#8217;s <code class="docutils literal"><span class="pre">profile_background_image_url</span></code>     should be tiled when displayed.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_background_tile&quot;: false
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>profile_banner_url</td>
<td>String</td>
<td><p class="first">The HTTPS-based URL pointing to the standard web representation of the user&#8217;s uploaded profile banner. By adding a final path element
of the URL, it is possible to obtain different image sizes optimized for specific displays.
For size variants, please see <a class="reference external" href="/basics/user-profile-images-and-banners">User Profile Images and Banners</a> .</p>
<p>Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_banner_url&quot;: &quot;https://si0.twimg.com/profile_banners/819797/1348102824&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>profile_image_url</td>
<td>String</td>
<td><p class="first">A HTTP-based URL pointing to the user&#8217;s profile image.
See <a class="reference external" href="/basics/user-profile-images-and-banners">User Profile Images and Banners</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_image_url&quot;:
&quot;http://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>profile_image_url_https</td>
<td>String</td>
<td><p class="first">A HTTPS-based URL pointing to the user&#8217;s profile image.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_image_url_https&quot;:
&quot;https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>profile_link_color</td>
<td>String</td>
<td><p class="first">The hexadecimal color the user has chosen to display links with in their Twitter UI.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_link_color&quot;: &quot;0094C2&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>profile_sidebar_border_color</td>
<td>String</td>
<td><p class="first">The hexadecimal color the user has chosen to display sidebar borders with in their Twitter UI.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_sidebar_border_color&quot;: &quot;0094C2&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>profile_sidebar_fill_color</td>
<td>String</td>
<td><p class="first">The hexadecimal color the user has chosen to display sidebar backgrounds with in their Twitter UI.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_sidebar_fill_color&quot;: &quot;a9d9f1&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-even"><td>profile_text_color</td>
<td>String</td>
<td><p class="first">The hexadecimal color the user has chosen to display text with in their Twitter UI.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_text_color&quot;: &quot;437792&quot;
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>profile_use_background_image</td>
<td>Boolean</td>
<td><p class="first">When true, indicates the user wants their uploaded background image to be used.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;profile_use_background_image&quot;: true
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>default_profile</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that the user has not altered the theme or background of their user profile.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;default_profile&quot;: false
</pre></div>
</div>
</td>
</tr>
<tr class="row-odd"><td>default_profile_image</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that the user has not uploaded their own profile image and a default image is used instead.
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;default_profile_image&quot;: false
</pre></div>
</div>
</td>
</tr>




</tbody>
</table>

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



## Deprecated Attributes

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

<tr class="row-even"><td>is_translator</td>
<td>Boolean</td>
<td><p class="first">When true, indicates that the user is a participant in Twitter&#8217;s <a class="reference external" href="http://translate.twitter.com">translator community</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;is_translator&quot;: false
</pre></div>
</div>
</td>
</tr>

<tr class="row-even"><td>following</td>
<td>Type</td>
<td><p class="first"><em>Nullable</em> . <em>Perspectival</em> . <em>Deprecated.</em> When true, indicates that the authenticating user is following this user. Some false
negatives are possible when set to &#8220;false,&#8221; but these false negatives are increasingly being represented as &#8220;null&#8221; instead. See
<a class="reference external" href="http://groups.google.com/group/twitter-development-talk/browse_thread/thread/42ba883b9f8e3c6e?tvc=2">Discussion</a> .
Example:</p>
<div class="code javascript last highlight-python"><div class="highlight"><pre><span></span>&quot;following&quot;: true
</pre></div>
</div>
</td>
</tr>

<tr class="row-odd"><td>notifications</td>
<td>Boolean</td>
<td><em>Nullable</em> . <em>Deprecated.</em> May incorrectly report &#8220;false&#8221; at times. Indicates whether the authenticated user has chosen to receive
this user&#8217;s Tweets by SMS.
<a class="reference external" href="http://groups.google.com/group/twitter-development-talk/browse_thread/thread/42ba883b9f8e3c6e?tvc=2">Discussion</a></td>
</tr>


</tbody>
</table>


