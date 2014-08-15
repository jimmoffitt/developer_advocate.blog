
###Data Loader

Notes mainly.





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


####Ruby code for loading into MySQL database:

```


require_relative './data_store.rb'
require 'csv'

    #Make a database connection.
    oDS = DataStore.new
    oDS.getSystemConfig('./config.yaml') #<--- schema, host, port, authentication
    oDS.establish_connection

    #Import CSV data as hashes
    sites = {}
    sites = getHashes(FILE_ROOT + "/BoulderFlood/WeatherData/SiteMetadata.csv")
   
    sites.each do |item|
        #oDS.storeSite item[0], item[1]
    end
```

```
#
#Builds an array of hashes...
#{10017=>{:name=>"James Creek", :lat=>40.11, :long=>-105.38, :altitude=>5842}, 10018=>{:name=>"Lower Lefthand", :lat=>40.126, :long=>-105.30, :altitude=>6230}}
#
def getHashes(file)
    #Create sites hash
    hash = {}
    p "Reading #{file}..."
    csv = CSV.read(file, :headers => true, :header_converters => :symbol, :converters => :all)

    p "Creating hash for  #{file}..."
    csv.each do |row|
        hash[row.fields[0]] = Hash[row.headers[1..-1].zip(row.fields[1..-1])]
    end

    hash
end
```
