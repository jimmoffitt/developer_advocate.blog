
###General themes

+ Audience? anyone interested in metadata exchange. use-case: Hydrology community.
+ Fundamental issues: data privacy, data interpretation, public safety
     + Privacy issues drive these questions: Human readable?  Use of Direct Messages?
+ Fundamental concepts: leveraging network, lack of latency, global reach


####Data transfer model
Systems will poll every 10 minutes, then defined triggers will start streaming.

Twitter public network/internet will provide a primary broadcast channel, but not the only one.
Standard and mission-critical backups still apply: ALERT radio, satellite, celluar

Systems could tweet any time there are metadata updates (new sites, new sensors).
Request ---> Response mechanism readily available.

Interested systems could request updates once a day.

Step 1: posting of data
Step 2: requesting cross-system metadata (public)
Step 3: requesting cross-system data (private/public)



Data Request Hashtags:
```
#NewSite
#NewSensor
#SiteMaintenance
#SensorMaintenance
#SendSiteSummary
#SendSensorSummary #Temp #Rain
```

###Payload characters

+ Sensor type: 16
+ Sensor ID: 16
+ Sensor time: 23    "YYYY-MM-DD HH:MM:SS TZZ"
+ Sensor Data: 16
+ Sensor Data 2: 16

=======================
Total characters: 87 (Max + whitespace)



###System Actor


```
"actor": {
    "objectType": "system",
    "id": "id:twitter.com:1855784545",
    "link": "http:\/\/www.twitter.com\/LakeCountyFCD",
    "displayName": "Lake County Flood Control District",
    "postedTime": "2014-07-01T00:00:00.000Z",
    "image": "https:\/\/pbs.twimg.com\/profile_images\/378800000646150423\/83090ccb95a60def923c674e7bd002a0_normal.jpeg",
    "summary": "Sensor Network used for weather and hydrology monitoring and flood warning.",
    "links": [
      {
        "href": null,
        "rel": "me"
      }
    ],
    "friendsCount": 0,
    "followersCount": 0,
    "listedCount": 0,
    "statusesCount": 0,
    "verified": true,
    "utcOffset": "-25200",
    "twitterTimeZone": "Mountain Time (US & Canada)",
    "preferredUsername": "LakeCountyFCD",
    "languages": [
      "en"
    ],
    "location": {
      "objectType": "place",
      "displayName": "Lake County, CO"
    },
    "favoritesCount": 0
  }
  
```
 
###Site Actor


```
"actor": {
    "objectType": "site",
    "id": "id:twitter.com:12400000",
    "link": "http:\/\/www.twitter.com\/LCFCD-LakeHarriet",
    "displayName": "BLake Harriet Weather Station",
    "postedTime": "2014-07-01T00:00:00.000Z",
    "image": "https:\/\/pbs.twimg.com\/profile_images\/378800000646150423\/83090ccb95a60def923c674e7bd002a0_normal.jpeg",
    "summary": "Temperature, Rain, Stage, Wind Speed, Wind Direction.",
    "links": [
      {
        "href": null,
        "rel": "me"
      }
    ],
    "friendsCount": 0,
    "followersCount": 0,
    "listedCount": 0,
    "statusesCount": 0,
    "verified": true,
    "utcOffset": "-25200",
    "twitterTimeZone": "Mountain Time (US & Canada)",
    "preferredUsername": "LakeHarriet",
    "languages": [
      "en"
    ],
    "location": {
      "objectType": "place",
      "displayName": "Lake Harriet, CO, -105.5689, 45.4598"
    },
    "favoritesCount": 0
  }
```



###Site Metadata Tweet

```
Name: Lake Harriet
ID: 1500
Lat: 45.4598
Long: -105.5689
Sensor Names: Rain, Count, Stage, Flow, Temp, WindDir, WindVel
Sensor Types: 10, 11
Sensor ID: 1500, 1510, 1520, 1560, 1570, 1571
```

###Sensor reports:


Temperature 
```
Temp: 70F 
Temp: 28C
ID: 1560
2014-07-11 09:45:00 MDT
```

Rain 
```
Rain: 0.04 in increment 
0.78 in 15-min | 1.45 1-hour | 2.56 3-hour
ID: 1500
2014-07-11 09:34:17 MDT
Count: 78
```

Stage
```
Stage: 6.5 ft
Flow: 4590 CFS (measured)
ID: 1510
2014-07-11 09:42:44 MDT
```







