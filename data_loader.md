
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

    def initialize(host=nil, port=nil, database=nil, user_name=nil, password=nil)
        #local database for storing activity data...

        @database_adapter = 'mysql'

        if host.nil? then
            @db_host = "127.0.0.1" #Local host is default.
        else
            @db_host = host
        end

        if port.nil? then
            @db_port = 3306 #MySQL post is default.
        else
            @db_port = port
        end

        if not user_name.nil?  #No default for this setting.
            @db_username = user_name
        end

        if not password.nil? #No default for this setting.
            @db_password = password
        end

        if not database.nil? #No default for this setting.
            @db_schema = database
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
