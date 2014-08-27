
Two JSON formats so far:

* Standard
* GeoJSON

###Standard

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

###GeoJSON

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
