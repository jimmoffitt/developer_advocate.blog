##Planning for High-Volume Social Data Events

Major national and global events are often accompanied by dramatic spikes in user activity across social media platforms. Sometimes these events are known about in advance, like the Super Bowl, political elections, and New Year’s celebrations around the world. Other times, the spikes in volume are due to unexpected happenings such as natural disasters, unplanned political events, or surprise pop culture moments like Ellen’s famous selfie Tweet at the Oscars. 

These bursts of user activity can be short-lived (measured in seconds) or they may be sustained over several minutes’ time. No matter their origin, it is important to consider the impact that they can have on applications consuming realtime social data from Gnip.  

Here are some best practices that will help your team prepare for high-volume social data events.

* Review Your Current PowerTrack Rules
     * Certain keywords can skyrocket during high volume events, for instance brand mentions when a brand sponsors a major sporting event
     * Be careful to avoid any unnecessary or overly generic PowerTrack rules that may generate unnecessary activity volumes
     * Consider communicating with your clients prior to known high-volume events to help them plan appropriately
* Stress Test Your Application
     * Anticipate that burst volumes may reach 5-10x average daily consumption levels. Depending on your PowerTrack rule set, the increase may be much higher.
* Optimize to Stay Connected
     * With realtime streams, staying connected is essential to avoid missing data. Your client application should be able to detect a disconnect and have logic to immediately retry its connection, using an exponential backoff if the reconnect attempt fails. 
* Add Built-In Buffering on Your End
     * Building a multi-threaded application is a key strategy for handling high-volume streams. At a high-level, a best practice for managing data streams is to have a separate thread/process that establishes the streaming connection and then writes received JSON activities to a memory structure or a buffered stream reader. This ‘light-weight’ stream processing thread is responsible for handling incoming data, which can be buffered in memory, growing and shrinking as needed. Then a different thread consumes that hash and does the ‘heavy lifting’ of parsing the JSON, preparing database writes, or whatever else your application needs to do.  
* Optional Streaming Data Recovery Tools
     * PowerTrack Replay is available to recover missed activities should you experience an extended disconnection.
     * PowerTrack Backfill automates data recovery if you disconnect briefly. If you disconnect and reconnect within 5 minutes, your data will be buffered by Gnip and delivered automatically.
     * If you are unsure whether your Gnip package includes these recovery features, be sure to contact your Account Manager to learn more.
* Considerations for Search API
     * It is possible for data indexing and data availability to become out of sync during a high load event.  If a “next” token is included in your response, it can be used to request additional results until you have sufficient data. This includes cases where the original response includes a “next” token and fewer Tweets than expected.
* Keep in Mind that Global Events = Global Time Zones
     * The events may occur after business hours or over the weekend, so be sure that your team is prepared for spikes to occur outside your normal business hours
* Don’t Panic!
     * As always, we recommend that you maintain your connections to Gnip services and monitor for any changes in delivery latency.
     * Gnip’s highly-scalable infrastructure ensures that none of your data will be lost or missed from any temporary increases in this latency.
     * The Gnip Support Team is always available if you have any issues and can be contacted via support@gnip.com.
