
Two JSON formats: 'Standard', GeoJSON
Two time-series intervals: 15-minute, 60-minute

* [Standard 60-minute]()
* [Standard 15-minute]()

* [GeoJSON 60-minute]()
* [GeoJSON 15-minute]()




##Standard

###Standard 60-minute interval

```
{
  "sites": {
    "stage": [
      {
        "site_id": "4410",
        "name": "Fourmile",
        "lat": "40.05",
        "long": "-105.369",
        "altitude": "6520"
      },
      {
        "site_id": "4580",
        "name": "Broadway",
        "lat": "40.0148889",
        "long": "-105.2785556",
        "altitude": "5348"
      }
    ],
    "rain": [
      {
        "site_id": "4050",
        "name": "Walker Ranch",
        "lat": "39.952",
        "long": "-105.339",
        "altitude": "7320"
      },
      {
        "site_id": "4070",
        "name": "Bear Peak",
        "lat": "39.971",
        "long": "-105.308",
        "altitude": "7486"
      }
    ]
  },
  "time_series": {
    "metadata": {
      "time_start": "2013-09-14 00:00:00",
      "time_end": "2013-09-14 02:00:00",
      "interval_minutes": 60,
      "time_format": "YYYY-MM-DD HH:mm:ss",
      "tz": "UTC",
      "tz_offset": 0
    },
    "interval_data": {
      "2013-09-14 01:00:00": {
        "stage": {
          "Fourmile": "2.02",
          "Broadway": "5.7"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0.000",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0.000",
            "accumulation": "11.37"
          }
        },
        "tweets": [
          {
            "id": "378670195101147136",
            "user_id": "29768489",
            "body": "It's not raining for now...\\nBut those clouds... #scary #flood #rain #colorado #weather @ Discount Tire http:\/\/t.co\/EzHJg0Lv6K",
            "lat": "39.5584",
            "long": "-104.885",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOFANYQpBY\/"
          },
          {
            "id": "378671980654129152",
            "user_id": "105204345",
            "body": "#boulderflood #100yearflood @ Boulder Farmers' Market http:\/\/t.co\/zIF5mkhntA",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOF4d3t24g\/"
          },
          {
            "id": "378674125067538432",
            "user_id": "755990293",
            "body": "NBC Coorespondent Miguel Almaguer from the flood zone tonight. #nbcnewspics #nbcnews #coloradoflood\ufffd\ufffd\ufffd http:\/\/t.co\/DL7Jwduufi",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOGUVsMadn\/"
          },
          {
            "id": "378676084835102720",
            "user_id": "12949002",
            "body": "Trailhead near my home post-flood @ Coal Creek Traihead (s. Public Rd.) http:\/\/t.co\/GRMWdoo5rL",
            "lat": "39.9576",
            "long": "-105.157",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOHmOyrlEG\/"
          },
          {
            "id": "378682226311192576",
            "user_id": "17342986",
            "body": "#Better #BoulderFlood @ Gulf of Orr http:\/\/t.co\/ODaKbp9GRD",
            "lat": "39.6143",
            "long": "-105.11",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOKg_-LKFZ\/"
          }
        ]
      },
      "2013-09-14 02:00:00": {
        "stage": {
          "Fourmile": "1.99",
          "Broadway": "5.51"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0.000",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0.000",
            "accumulation": "11.37"
          }
        },
        "tweets": [
          {
            "id": "378687087199129600",
            "user_id": "105954742",
            "body": "Colorado sky.  \ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd for the people affected by the flooding. @ Westminster, Colorado http:\/\/t.co\/Tt9OelLJqs",
            "lat": "39.8178",
            "long": "-105.166",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOMkQqlF52\/"
          },
          {
            "id": "378688365635567616",
            "user_id": "16343091",
            "body": "#clouds #sky #boulderflood @ Boulder, Colorado http:\/\/t.co\/XaXtcWRDpk",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eONS1ymDIi\/"
          },
          {
            "id": "378688813134249985",
            "user_id": "311168948",
            "body": "Sister Sink time #boulder #nomorerain #100yearflood @ The Sink http:\/\/t.co\/fOATZkLJxh",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eONiNDrkc6\/"
          },
          {
            "id": "378689230996013057",
            "user_id": "37132429",
            "body": "Stay safe tonight boulder #boulderflood @ US-36 at Table Mesa http:\/\/t.co\/P6TnhYgQ4L",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eONrAeGtaD\/"
          },
          {
            "id": "378689569228861440",
            "user_id": "90646865",
            "body": "Poor guy was so stir crazy from being inside with all the rain he immediately ran 6 laps, collapsed,\ufffd\ufffd\ufffd http:\/\/t.co\/NsH1lzrjXW",
            "lat": "39.6143",
            "long": "-105.11",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eONw_sEf8S\/"
          },
          {
            "id": "378689989183545344",
            "user_id": "559032601",
            "body": "Rise above the storm and you will find sunshine. \ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd @ Colorado http:\/\/t.co\/OYdtwVT7uU",
            "lat": "39.8891",
            "long": "-105.166",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eON9n6M5UW\/"
          },
          {
            "id": "378691190776164352",
            "user_id": "612949732",
            "body": "Boulder River.....I mean creek #boulderflood #boulder #cuboulder @ University Village at Boulder Creek\ufffd\ufffd\ufffd http:\/\/t.co\/H0Cxex1YqS",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOOm4umo_Y\/"
          },
          {
            "id": "378691923449765888",
            "user_id": "15737449",
            "body": "Flooded area in Boulder @ Boulder http:\/\/t.co\/QuHf6gPWHC",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eONA9EJZL5\/"
          },
          {
            "id": "378692493225000960",
            "user_id": "755990293",
            "body": "A normal Colorado sky hangs over Boulder tonight.  Forecasters say expect more rain. #boulderflood\ufffd\ufffd\ufffd http:\/\/t.co\/3smTIeVzcr",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOOIP8saZk\/"
          },
          {
            "id": "378694657976594432",
            "user_id": "29089844",
            "body": "Blackhawks over Boulder. #boulderflood #rescue @ Parkside Park http:\/\/t.co\/cIwlJbXYwU",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOQEsynxbR\/"
          },
          {
            "id": "378694958439751680",
            "user_id": "80984365",
            "body": "The calm after the storm \ufffd\ufffd\ufffd #nofilter #boulder #clouds @ The Palace http:\/\/t.co\/DKAxIA2DI7",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eONZPGBvPU\/"
          },
          {
            "id": "378699358499524608",
            "user_id": "25890794",
            "body": "Schoolyard in north Boulder filled w debris from the floods. #seriouscleanup #boulderflood @ Crest\ufffd\ufffd\ufffd http:\/\/t.co\/Rxm9eWxVgc",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOSM8twFeQ\/"
          }
        ]
      }
    }
  }
}
```

###Standard 15-minute interval

```
{
  "sites": {
    "stage": [
      {
        "site_id": "4410",
        "name": "Fourmile",
        "lat": "40.05",
        "long": "-105.369",
        "altitude": "6520"
      },
      {
        "site_id": "4580",
        "name": "Broadway",
        "lat": "40.0148889",
        "long": "-105.2785556",
        "altitude": "5348"
      }
    ],
    "rain": [
      {
        "site_id": "4050",
        "name": "Walker Ranch",
        "lat": "39.952",
        "long": "-105.339",
        "altitude": "7320"
      },
      {
        "site_id": "4070",
        "name": "Bear Peak",
        "lat": "39.971",
        "long": "-105.308",
        "altitude": "7486"
      }
    ]
  },
  "time_series": {
    "metadata": {
      "time_start": "2013-09-14 00:00:00",
      "time_end": "2013-09-14 00:30:00",
      "interval_minutes": 15,
      "time_format": "YYYY-MM-DD HH:mm:ss",
      "tz": "UTC",
      "tz_offset": 0
    },
    "interval_data": {
      "2013-09-14 00:15:00": {
        "stage": {
          "Fourmile": "2.02",
          "Broadway": "5.7"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0",
            "accumulation": "11.37"
          }
        },
        "tweets": [
          {
            "id": "378670195101147136",
            "user_id": "29768489",
            "body": "It's not raining for now...\\nBut those clouds... #scary #flood #rain #colorado #weather @ Discount Tire http:\/\/t.co\/EzHJg0Lv6K",
            "lat": "39.5584",
            "long": "-104.885",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOFANYQpBY\/"
          },
          {
            "id": "378671980654129152",
            "user_id": "105204345",
            "body": "#boulderflood #100yearflood @ Boulder Farmers' Market http:\/\/t.co\/zIF5mkhntA",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOF4d3t24g\/"
          }
        ]
      },
      "2013-09-14 00:30:00": {
        "stage": {
          "Fourmile": "2.01",
          "Broadway": "5.51"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0",
            "accumulation": "11.37"
          }
        },
        "tweets": [
          {
            "id": "378674125067538432",
            "user_id": "755990293",
            "body": "NBC Coorespondent Miguel Almaguer from the flood zone tonight. #nbcnewspics #nbcnews #coloradoflood\ufffd\ufffd\ufffd http:\/\/t.co\/DL7Jwduufi",
            "lat": "39.9641",
            "long": "-105.302",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOGUVsMadn\/"
          },
          {
            "id": "378676084835102720",
            "user_id": "12949002",
            "body": "Trailhead near my home post-flood @ Coal Creek Traihead (s. Public Rd.) http:\/\/t.co\/GRMWdoo5rL",
            "lat": "39.9576",
            "long": "-105.157",
            "media": null,
            "urls": "http:\/\/instagram.com\/p\/eOHmOyrlEG\/"
          }
        ]
      }
    }
  }
}

```

##GeoJSON

###GeoJSON 60-minute interval

```
{
  "sites": {
    "stage": [
      {
        "site_id": "4410",
        "name": "Fourmile",
        "lat": "40.05",
        "long": "-105.369",
        "altitude": "6520"
      },
      {
        "site_id": "4580",
        "name": "Broadway",
        "lat": "40.0148889",
        "long": "-105.2785556",
        "altitude": "5348"
      }
    ],
    "rain": [
      {
        "site_id": "4050",
        "name": "Walker Ranch",
        "lat": "39.952",
        "long": "-105.339",
        "altitude": "7320"
      },
      {
        "site_id": "4070",
        "name": "Bear Peak",
        "lat": "39.971",
        "long": "-105.308",
        "altitude": "7486"
      }
    ]
  },
  "time_series": {
    "metadata": {
      "time_start": "2013-09-14 00:00:00",
      "time_end": "2013-09-14 02:00:00",
      "interval_minutes": 60,
      "time_format": "YYYY-MM-DD HH:mm:ss",
      "tz": "UTC",
      "tz_offset": 0
    },
    "interval_data": {
      "2013-09-14 01:00:00": {
        "stage": {
          "Fourmile": "2.02",
          "Broadway": "5.7",
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0.000",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0.000",
            "accumulation": "11.37"
          }
        },
        "tweets": {
          "type": "FeatureCollection",
          "features": [
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-104.885",
                  "39.5584"
                ]
              },
              "properties": {
                "body": "It's not raining for now...\\nBut those clouds... #scary #flood #rain #colorado #weather @ Discount Tire http:\/\/t.co\/EzHJg0Lv6K",
                "link": "http:\/\/instagram.com\/p\/eOFANYQpBY\/",
                "tweet": "http:\/\/twitter.com\/JMAC303\/statuses\/378670195101147136",
                "user_id": "29768489"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "#boulderflood #100yearflood @ Boulder Farmers' Market http:\/\/t.co\/zIF5mkhntA",
                "link": "http:\/\/instagram.com\/p\/eOF4d3t24g\/",
                "tweet": "http:\/\/twitter.com\/ajonesdesign\/statuses\/378671980654129152",
                "user_id": "105204345"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "NBC Coorespondent Miguel Almaguer from the flood zone tonight. #nbcnewspics #nbcnews #coloradoflood\ufffd\ufffd\ufffd http:\/\/t.co\/DL7Jwduufi",
                "link": "http:\/\/instagram.com\/p\/eOGUVsMadn\/",
                "tweet": "http:\/\/twitter.com\/rayfarmer_\/statuses\/378674125067538432",
                "user_id": "755990293"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.157",
                  "39.9576"
                ]
              },
              "properties": {
                "body": "Trailhead near my home post-flood @ Coal Creek Traihead (s. Public Rd.) http:\/\/t.co\/GRMWdoo5rL",
                "link": "http:\/\/instagram.com\/p\/eOHmOyrlEG\/",
                "tweet": "http:\/\/twitter.com\/dahle\/statuses\/378676084835102720",
                "user_id": "12949002"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.11",
                  "39.6143"
                ]
              },
              "properties": {
                "body": "#Better #BoulderFlood @ Gulf of Orr http:\/\/t.co\/ODaKbp9GRD",
                "link": "http:\/\/instagram.com\/p\/eOKg_-LKFZ\/",
                "tweet": "http:\/\/twitter.com\/303BryanFleming\/statuses\/378682226311192576",
                "user_id": "17342986"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-107.871",
                  "37.9323"
                ]
              },
              "properties": {
                "body": "The mountain looks angry. #weather #mountain #rain #sound #speakers @ Telluride Blues and Brews Festival http:\/\/t.co\/WUrcc4CtwX",
                "link": "http:\/\/instagram.com\/p\/eOK39cFfjm\/",
                "tweet": "http:\/\/twitter.com\/mikeyt6969\/statuses\/378683541917544449",
                "user_id": "211599291"
              }
            }
          ]
        }
      },
      "2013-09-14 02:00:00": {
        "stage": {
          "Fourmile": "1.99",
          "Broadway": "5.51"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0.000",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0.000",
            "accumulation": "11.37"
          }
        },
        "tweets": {
          "type": "FeatureCollection",
          "features": [
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.166",
                  "39.8178"
                ]
              },
              "properties": {
                "body": "Colorado sky.  \ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd for the people affected by the flooding. @ Westminster, Colorado http:\/\/t.co\/Tt9OelLJqs",
                "link": "http:\/\/instagram.com\/p\/eOMkQqlF52\/",
                "tweet": "http:\/\/twitter.com\/FloralOccasions\/statuses\/378687087199129600",
                "user_id": "105954742"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "#clouds #sky #boulderflood @ Boulder, Colorado http:\/\/t.co\/XaXtcWRDpk",
                "link": "http:\/\/instagram.com\/p\/eONS1ymDIi\/",
                "tweet": "http:\/\/twitter.com\/its_Leah\/statuses\/378688365635567616",
                "user_id": "16343091"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "Sister Sink time #boulder #nomorerain #100yearflood @ The Sink http:\/\/t.co\/fOATZkLJxh",
                "link": "http:\/\/instagram.com\/p\/eONiNDrkc6\/",
                "tweet": "http:\/\/twitter.com\/katiejosells\/statuses\/378688813134249985",
                "user_id": "311168948"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "Stay safe tonight boulder #boulderflood @ US-36 at Table Mesa http:\/\/t.co\/P6TnhYgQ4L",
                "link": "http:\/\/instagram.com\/p\/eONrAeGtaD\/",
                "tweet": "http:\/\/twitter.com\/nikitafedorov\/statuses\/378689230996013057",
                "user_id": "37132429"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.11",
                  "39.6143"
                ]
              },
              "properties": {
                "body": "Poor guy was so stir crazy from being inside with all the rain he immediately ran 6 laps, collapsed,\ufffd\ufffd\ufffd http:\/\/t.co\/NsH1lzrjXW",
                "link": "http:\/\/instagram.com\/p\/eONw_sEf8S\/",
                "tweet": "http:\/\/twitter.com\/bodhi_bear\/statuses\/378689569228861440",
                "user_id": "90646865"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.166",
                  "39.8891"
                ]
              },
              "properties": {
                "body": "Rise above the storm and you will find sunshine. \ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd\ufffd @ Colorado http:\/\/t.co\/OYdtwVT7uU",
                "link": "http:\/\/instagram.com\/p\/eON9n6M5UW\/",
                "tweet": "http:\/\/twitter.com\/NRSantangelo_11\/statuses\/378689989183545344",
                "user_id": "559032601"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "Boulder River.....I mean creek #boulderflood #boulder #cuboulder @ University Village at Boulder Creek\ufffd\ufffd\ufffd http:\/\/t.co\/H0Cxex1YqS",
                "link": "http:\/\/instagram.com\/p\/eOOm4umo_Y\/",
                "tweet": "http:\/\/twitter.com\/bthompson4119\/statuses\/378691190776164352",
                "user_id": "612949732"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "Flooded area in Boulder @ Boulder http:\/\/t.co\/QuHf6gPWHC",
                "link": "http:\/\/instagram.com\/p\/eONA9EJZL5\/",
                "tweet": "http:\/\/twitter.com\/tom_fitz\/statuses\/378691923449765888",
                "user_id": "15737449"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "A normal Colorado sky hangs over Boulder tonight.  Forecasters say expect more rain. #boulderflood\ufffd\ufffd\ufffd http:\/\/t.co\/3smTIeVzcr",
                "link": "http:\/\/instagram.com\/p\/eOOIP8saZk\/",
                "tweet": "http:\/\/twitter.com\/rayfarmer_\/statuses\/378692493225000960",
                "user_id": "755990293"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "Blackhawks over Boulder. #boulderflood #rescue @ Parkside Park http:\/\/t.co\/cIwlJbXYwU",
                "link": "http:\/\/instagram.com\/p\/eOQEsynxbR\/",
                "tweet": "http:\/\/twitter.com\/timfalls\/statuses\/378694657976594432",
                "user_id": "29089844"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "The calm after the storm \ufffd\ufffd\ufffd #nofilter #boulder #clouds @ The Palace http:\/\/t.co\/DKAxIA2DI7",
                "link": "http:\/\/instagram.com\/p\/eONZPGBvPU\/",
                "tweet": "http:\/\/twitter.com\/Em_Russ\/statuses\/378694958439751680",
                "user_id": "80984365"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "Schoolyard in north Boulder filled w debris from the floods. #seriouscleanup #boulderflood @ Crest\ufffd\ufffd\ufffd http:\/\/t.co\/Rxm9eWxVgc",
                "link": "http:\/\/instagram.com\/p\/eOSM8twFeQ\/",
                "tweet": "http:\/\/twitter.com\/BeBeingBecome\/statuses\/378699358499524608",
                "user_id": "25890794"
              }
            }
          ]
        }
      }
    }
  }
}

```

###GeoJSON 15-minute interval

```
{
  "sites": {
    "stage": [
      {
        "site_id": "4410",
        "name": "Fourmile",
        "lat": "40.05",
        "long": "-105.369",
        "altitude": "6520"
      },
      {
        "site_id": "4580",
        "name": "Broadway",
        "lat": "40.0148889",
        "long": "-105.2785556",
        "altitude": "5348"
      }
    ],
    "rain": [
      {
        "site_id": "4050",
        "name": "Walker Ranch",
        "lat": "39.952",
        "long": "-105.339",
        "altitude": "7320"
      },
      {
        "site_id": "4070",
        "name": "Bear Peak",
        "lat": "39.971",
        "long": "-105.308",
        "altitude": "7486"
      }
    ]
  },
  "time_series": {
    "metadata": {
      "time_start": "2013-09-14 00:00:00",
      "time_end": "2013-09-14 00:30:00",
      "interval_minutes": 15,
      "time_format": "YYYY-MM-DD HH:mm:ss",
      "tz": "UTC",
      "tz_offset": 0
    },
    "interval_data": {
      "2013-09-14 00:15:00": {
        "stage": {
          "Fourmile": "2.02",
          "Broadway": "5.7"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0",
            "accumulation": "11.37"
          }
        },
        "tweets": {
          "type": "FeatureCollection",
          "features": [
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-104.885",
                  "39.5584"
                ]
              },
              "properties": {
                "body": "It's not raining for now...\\nBut those clouds... #scary #flood #rain #colorado #weather @ Discount Tire http:\/\/t.co\/EzHJg0Lv6K",
                "link": "http:\/\/instagram.com\/p\/eOFANYQpBY\/",
                "tweet": "http:\/\/twitter.com\/JMAC303\/statuses\/378670195101147136",
                "user_id": "29768489"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "#boulderflood #100yearflood @ Boulder Farmers' Market http:\/\/t.co\/zIF5mkhntA",
                "link": "http:\/\/instagram.com\/p\/eOF4d3t24g\/",
                "tweet": "http:\/\/twitter.com\/ajonesdesign\/statuses\/378671980654129152",
                "user_id": "105204345"
              }
            }
          ]
        }
      },
      "2013-09-14 00:30:00": {
        "stage": {
          "Fourmile": "2.01",
          "Broadway": "5.51"
        },
        "rain": {
          "Walker Ranch": {
            "increment": "0",
            "accumulation": "8.44"
          },
          "Bear Peak": {
            "increment": "0",
            "accumulation": "11.37"
          }
        },
        "tweets": {
          "type": "FeatureCollection",
          "features": [
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.302",
                  "39.9641"
                ]
              },
              "properties": {
                "body": "NBC Coorespondent Miguel Almaguer from the flood zone tonight. #nbcnewspics #nbcnews #coloradoflood\ufffd\ufffd\ufffd http:\/\/t.co\/DL7Jwduufi",
                "link": "http:\/\/instagram.com\/p\/eOGUVsMadn\/",
                "tweet": "http:\/\/twitter.com\/rayfarmer_\/statuses\/378674125067538432",
                "user_id": "755990293"
              }
            },
            {
              "type": "Feature",
              "geometry": {
                "type": "Point",
                "coordinate": [
                  "-105.157",
                  "39.9576"
                ]
              },
              "properties": {
                "body": "Trailhead near my home post-flood @ Coal Creek Traihead (s. Public Rd.) http:\/\/t.co\/GRMWdoo5rL",
                "link": "http:\/\/instagram.com\/p\/eOHmOyrlEG\/",
                "tweet": "http:\/\/twitter.com\/dahle\/statuses\/378676084835102720",
                "user_id": "12949002"
              }
            }
          ]
        }
      }
    }
  }
}

```
