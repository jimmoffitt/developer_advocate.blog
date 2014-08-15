
##Data Loader

Notes mainly. Includes code for transforming a CSV file into a hash. Also, some ActiveRecord code for writing to an existing MySQL database.


###Site metadata.

Site Metadata in a simple CSV format: site_id, name, lat, long, altitude

###Loading weather station site metadata

Data processed was produced by OneRain's Insight app.

SiteMetadata.csv

```
site_id,name,lat,long,altitude
10018,Lower Lefthand,40.1263880,-105.3044860,6230
10021,S. St. Vrain at Berry Rdg,40.1675500,-105.3947670,6564
4050,Walker Ranch,39.9520000,-105.3390000,7320
4110,Betasso,40.0240000,-105.3450000,6480
4240,Sunset,40.0490000,-105.4720000,8680
4390,Boulder Falls,40.0030000,-105.4000000,6820
4410,Fourmile,40.0500000,-105.3690000,6520
4580,Broadway,40.0148889,-105.2785556,5348
```


```
require_relative './data_store.rb'
require 'csv'

    #Make a database connection.
    oDS = DataStore.new
    oDS.getSystemConfig('./config.yaml') #<--- schema, host, port, authentication
    oDS.establish_connection

    #Import CSV data as hashes
    sites = {}
    sites = getHashes(FILE_ROOT + "/BoulderFlood/WeatherData/SiteMetadata.csv") #See code below.
   
    sites.each do |item|
        #oDS.storeSite item[0], item[1]  #See code below.
    end
```

This method takes a CSV file and builds an array of site hashes:

Turns
```
site_id,name,lat,long,altitude
10018,Lower Lefthand,40.1264,-105.3045,6230
4050,Walker Ranch,39.9520,-105.3390,7320
```

Into this JSON:
```
{
  10018=>{:name=>"Lower Lefthand", :lat=>40.1264, :long=>-105.3045, :altitude=>6230}
  4050=>{:name=>"Walker Ranch", :lat=39.9520, :long=>-105.3390, :altitude=>7320}
}
```

Here's the magic, just pass it the CSV file and get back the hash described above:

```
#Builds an array of hashes...
def getHashes(file)
    hash = {}
    csv = CSV.read(file, :headers => true, :header_converters => :symbol, :converters => :all)

    csv.each do |row|
        hash[row.fields[0]] = Hash[row.headers[1..-1].zip(row.fields[1..-1])]
    end

    hash
end
```

####Ruby code for loading into MySQL database:

Some code for writing to an existing MySQL database.

```
require 'active_record'
require "yaml"

class Site < ActiveRecord::Base
    validates_uniqueness_of :site_id
end

class DataStore
    attr_accessor :data_store, :database_adapter, :db_host, :db_port, :schema, :db_username, :db_password

    def getSystemConfig(config_file)

        config = YAML.load_file(config_file)

        @db_host = config["database"]["host"]
        @db_port = config["database"]["port"]
        @db_schema = config["database"]["schema"]
        @db_user_name = config["database"]["user_name"]
        @db_password  = config["database"]["password"]
    end

    def establish_connection
        begin
            ActiveRecord::Base.establish_connection(
                :adapter => @database_adapter,
                :host => @db_host,
                :username => @db_user_name,
                :password => @db_password,
                :database => @db_schema,
                :pool => 20,
                :timeout => 100000
            )
        rescue Exception => e
            p "Could not connect!: " + e.message
        end
    end

    def storeSite site_id, site
        begin
            #Reflects the sites schema: (you also get an auto-increment int via ActiveRecord.
            Site.create(:site_id => site_id,
                        :lat => site[:lat],
                        :long => site[:long],
                        :altitude => site[:altitude]
                       )
                #p "Site #{site_id} created."
        rescue Exception => e
            p e.message
        end
    end
end
```

###Processing time-series data

Time-series weather data was in the following format. 
* 15-minute time-steps (converted from local MDT to UTC).
* Timestamp represents the end of interval: 04:15 is the amount of rain between 4:00-4:15 UTC.

####Rain data

* Rain measures in inches.
* 15-minute rain increments.
* Event accumulation totals (zeroed at 00:00 MDT September 9) 

#####increments
```
timestamp,1110,1920,4050,4070,4090,4110,4140,4170,4220,4240,4360,4470,4490,4550,4730,4830,4870,6601
2013-09-12 04:15,0.43,0,0.12,0,0.08,0.16,0.16,0.08,0.32,0.08,0.04,0.55,0.12,0.32,0.12,0,0.24,0.1
2013-09-12 04:30,0.28,0,0.16,0.32,0.12,0.08,0.12,0.08,0,0,0.35,0,0.08,0.2,0.08,0,0.04,0.06
2013-09-12 04:45,0.35,0,0.12,0.35,0.2,0.39,0.08,0.24,0,0.12,0.47,0.24,0.24,0.32,0.08,0.04,0.04,0.04
2013-09-12 05:00,0.08,0.04,0.24,0.16,0.16,0,0.16,0.04,0,0.12,0.2,0,0.04,0.12,0.24,0.12,0.04,0.1
```

#####accumulations
```
timestamp,1110,1920,4050,4070,4090,4110,4140,4170,4220,4240,4360,4470,4490,4550,4730,4830,4870,6601
2013-09-12 04:15,3.67,2.23,4.48,5.66,5.04,5.03,4.56,4.12,3.75,3.07,5.24,4.14,3.52,4.15,3.56,6.15,6.37,3.97
2013-09-12 04:30,3.95,2.23,4.64,5.98,5.16,5.11,4.68,4.2,3.75,3.07,5.59,4.14,3.6,4.35,3.64,6.15,6.41,4.03
2013-09-12 04:45,4.3,2.23,4.76,6.33,5.36,5.5,4.76,4.44,3.75,3.19,6.06,4.38,3.84,4.67,3.72,6.19,6.45,4.07
2013-09-12 05:00,4.38,2.27,5,6.49,5.52,5.5,4.92,4.48,3.75,3.31,6.26,4.38,3.88,4.79,3.96,6.31,6.49,4.17
```

###Processing time-series stage data

* River levels in feet.
* 15-minute stage maximums and averages.

#####stage maximums
```
timestamp,10017,10018,10021,1110,4370,4380,4390,4400,4410,4420,4430,4470,4560,4580,4590,4760,4830,4870
2013-09-12 04:15,1.09,3.18,2.01,14.64,1.51,2.74,1.38,3.1,1.31,4.34,0.49,3.59,2.44,3.93,4.66,122.62,2.01,1.06
2013-09-12 04:30,1.23,3.24,2.65,14.86,1.62,2.74,1.38,3.1,1.51,4.62,0.49,4.91,2.44,4.46,4.74,122.62,2.17,1.75
2013-09-12 04:45,1.3,3.46,2.65,16.18,1.68,4.02,1.38,3.28,1.58,4.77,0.49,5.48,2.44,4.48,4.82,122.62,2.41,1.97
2013-09-12 05:00,1.44,3.57,2.9,16.18,1.91,4.27,1.6,3.33,1.59,5.06,0.49,6,2.44,4.56,4.82,122.62,2.72,2.18
```
#####Stage averages
```
timestamp,10017,10018,10021,1110,4370,4380,4390,4400,4410,4420,4430,4470,4560,4580,4580,4590,4760,4830,4870
2013-09-12 04:15,1.09,3.18,2.01,14.64,1.51,2.74,1.38,3.08,1.28,4.22,0.49,3.59,2.44,,3.93,4.66,122.62,1.96,1.06
2013-09-12 04:30,1.23,3.24,2.65,14.78,1.62,2.74,1.38,3.08,1.44,4.37,0.49,4.14,2.44,,4.43,4.74,122.62,2.17,1.75
2013-09-12 04:45,1.3,3.4,2.65,15.87,1.68,4.02,1.38,3.28,1.58,4.52,0.49,5.48,2.44,,4.48,4.82,122.62,2.34,1.97
2013-09-12 05:00,1.44,3.54,2.9,15.87,1.88,4.27,1.6,3.33,1.56,4.87,0.49,5.99,2.44,,4.56,4.82,122.62,2.62,2.09


