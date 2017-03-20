## Available Operators and matching JSON


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-baqh{text-align:center;vertical-align:top}
.tg .tg-a69i{font-weight:bold;background-color:#86baf4;text-align:center;vertical-align:top}
.tg .tg-nhkk{font-weight:bold;background-color:#86baf4;vertical-align:top}
.tg .tg-yw4l{vertical-align:top}
</style>

<table class="tg">
  <tr>
    <th class="tg-a69i"></th>
    <th class="tg-a69i"></th>
    <th class="tg-a69i"></th>
    <th class="tg-nhkk"></th>
    <th class="tg-a69i" colspan="2">JSON attribute</th>
    <th class="tg-nhkk"></th>
  </tr>
  <tr>
    <td class="tg-a69i">Operator</td>
    <td class="tg-a69i">PowerTrack (real-time and Historical)</td>
    <td class="tg-a69i">Search APIs (30-Day and Full-Archive)</td>
    <td class="tg-a69i">Matches on</td>
    <td class="tg-a69i">Native format</td>
    <td class="tg-a69i">Activity Stream format</td>
    <td class="tg-a69i">Comments</td>
  </tr>
  <tr>
    <td class="tg-baqh">"exact phrase match"</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"text"</td>
    <td class="tg-yw4l">"body"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">@</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter entities</td>
    <td class="tg-yw4l">"entities"."user_mentions"</td>
    <td class="tg-yw4l">"twitter_entities"."user_mentions"</td>
    <td class="tg-yw4l">User/Actor numeric IDs or Twitter handle</td>
  </tr>
  <tr>
    <td class="tg-baqh">#</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter entities</td>
    <td class="tg-yw4l">"entities"."hashtags"</td>
    <td class="tg-yw4l">"twitter_entities"."hashtags"</td>
    <td class="tg-yw4l">Hashtag matching</td>
  </tr>
  <tr>
    <td class="tg-baqh">$</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter entities</td>
    <td class="tg-yw4l">"entities"."symbols"</td>
    <td class="tg-yw4l">"twitter_entities"."symbols"</td>
    <td class="tg-yw4l">Cashtag matching</td>
  </tr>
  <tr>
    <td class="tg-baqh">bio:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user"."description"</td>
    <td class="tg-yw4l">"actor"."summary"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">bio_location:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"."displayName"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">bio_name:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"name"</td>
    <td class="tg-yw4l">"actor"."displayName"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">bounding_box:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Place object</td>
    <td class="tg-yw4l">"place"</td>
    <td class="tg-yw4l">"location"."geo"."coordinates"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">contains:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"text"</td>
    <td class="tg-yw4l">"body"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">&lt;emoji&gt;</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"text"</td>
    <td class="tg-yw4l">"body"</td>
    <td class="tg-yw4l">&lt;emoji&gt; replaced with actual emoji or 'character' code</td>
  </tr>
  <tr>
    <td class="tg-baqh">followers_count:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"followers_count"</td>
    <td class="tg-yw4l">"actor".followersCount"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">friends_count:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"friends_count"</td>
    <td class="tg-yw4l">"actor"."friendsCount"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">from:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet 'source' attribute</td>
    <td class="tg-yw4l">"user"."id", "user"."id_str", "user"."screen_name"</td>
    <td class="tg-yw4l">"actor"."id", "actor"."preferredUsername"</td>
    <td class="tg-yw4l">User/Actor numeric IDs or Twitter handle</td>
  </tr>
  <tr>
    <td class="tg-baqh">has:geo</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Place object</td>
    <td class="tg-yw4l">"place"</td>
    <td class="tg-yw4l">"location"."geo"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">has:hashtags</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"entities":"hashtags"</td>
    <td class="tg-yw4l">"twitter_entities"."hashtags"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">has:images</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter Entities</td>
    <td class="tg-yw4l">"entities":"media"</td>
    <td class="tg-yw4l">"twitter_entities"."media"</td>
    <td class="tg-yw4l">"type"."photo"</td>
  </tr>
  <tr>
    <td class="tg-baqh">has:lang</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Tweet received language classification</td>
    <td class="tg-yw4l">twitter_lang is not 'und'</td>
    <td class="tg-yw4l">"twitter_lang"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">has:links</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter Entities</td>
    <td class="tg-yw4l">"entities":"urls"</td>
    <td class="tg-yw4l">"twitter_entities"."urls"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">has:media</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter Entities</td>
    <td class="tg-yw4l">"entities":"media"</td>
    <td class="tg-yw4l">"twitter_entities"."media"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">has:mentions</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet entities</td>
    <td class="tg-yw4l">"entities"."user_mentions"</td>
    <td class="tg-yw4l">"twitter_entities".</td>
    <td class="tg-yw4l">Note about "twitter_extended_entities"</td>
  </tr>
  <tr>
    <td class="tg-baqh">has:profile_geo</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"."displayName"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">has:symbols</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter Entities</td>
    <td class="tg-yw4l">"entities"."symbols"</td>
    <td class="tg-yw4l">"twitter_entities"."symbols"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">has:videos</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter Entities</td>
    <td class="tg-yw4l">"entities"."media"</td>
    <td class="tg-yw4l">"twitter_entities"."media"</td>
    <td class="tg-yw4l">"type"."video"</td>
  </tr>
  <tr>
    <td class="tg-baqh">in_reply_to_status_id:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">is:quote</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">is:retweet</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet metadata</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l">"verb":"share"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">is:verified</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"verified"</td>
    <td class="tg-yw4l">"actor"."verified"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">keyword</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"text"</td>
    <td class="tg-yw4l">"body"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">lang:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet 'lang' attribute</td>
    <td class="tg-yw4l">"lang"</td>
    <td class="tg-yw4l">"twitter_lang"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">listed_count:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"listed_count"</td>
    <td class="tg-yw4l">"actor"."listedCount"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">place_country:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Place object</td>
    <td class="tg-yw4l">"place"</td>
    <td class="tg-yw4l">"location"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">place:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Place object</td>
    <td class="tg-yw4l">"place"</td>
    <td class="tg-yw4l">"location"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">point_radius:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Place object</td>
    <td class="tg-yw4l">"place"</td>
    <td class="tg-yw4l">"location"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">profile_bounding_box</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">profile_country:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">profile_locality:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">profile_point_radius</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">profile_region:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">profile_subregion:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"location"</td>
    <td class="tg-yw4l">"actor"."location"</td>
    <td class="tg-yw4l">Profile Geo enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">proximity~N</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"text"</td>
    <td class="tg-yw4l">"body"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">retweets_of_status_id:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l">Operand is a single Tweet numeric ID</td>
  </tr>
  <tr>
    <td class="tg-baqh">retweets_of:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet metadata</td>
    <td class="tg-yw4l">"retweeted_status" exists</td>
    <td class="tg-yw4l">"verb":"share"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">sample:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">N/A</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l">Description here...</td>
  </tr>
  <tr>
    <td class="tg-baqh">source:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Tweet metadata</td>
    <td class="tg-yw4l">"source"</td>
    <td class="tg-yw4l">"generator"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">statuses_count:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"statuses_count"</td>
    <td class="tg-yw4l">"actor":"statusesCount"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">time_zone:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">User object</td>
    <td class="tg-yw4l">"user":"time_zone"</td>
    <td class="tg-yw4l">"actor"."twitterTimeZone"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">to:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Tweet body</td>
    <td class="tg-yw4l">"text"</td>
    <td class="tg-yw4l">"body"</td>
    <td class="tg-yw4l">User/Actor numeric IDs or Twitter handle</td>
  </tr>
  <tr>
    <td class="tg-baqh">url_contains:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Twitter Entities /Gnip URLs</td>
    <td class="tg-yw4l">"entities"."urls"."expanded_url"</td>
    <td class="tg-yw4l">"twitter_entities"."urls"."expanded_url"  "gnip"."urls"."expanded_url"</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-baqh">url_description:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Twitter Entities / Gnip URLs</td>
    <td class="tg-yw4l">"entities"."urls"."unwound"."description"</td>
    <td class="tg-yw4l">"gnip"."urls"."expanded_url_description"</td>
    <td class="tg-yw4l">Enhanced URL enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">url_title:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh"></td>
    <td class="tg-yw4l">Twitter Entities / Gnip URLs</td>
    <td class="tg-yw4l">"entities"."urls"."unwound"."title"</td>
    <td class="tg-yw4l">"gnip"."urls"."expanded_url_title"</td>
    <td class="tg-yw4l">Enhanced URL enrichment</td>
  </tr>
  <tr>
    <td class="tg-baqh">url:</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-baqh">✔</td>
    <td class="tg-yw4l">Twitter Entities /Gnip URLs</td>
    <td class="tg-yw4l">"entities"."urls"."expanded_url"</td>
    <td class="tg-yw4l">"twitter_entities"."urls"."expanded_url"</td>
    <td class="tg-yw4l"></td>
  </tr>
</table>
