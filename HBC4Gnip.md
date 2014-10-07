+ [] Re-do all steps via command-line (without IDE)
+ [] Install Maven and compile code: $mvn compile
+ [] Package com.thinksnow.hbc
+ [] Command-line example run 


###Setting up Twitter Hosebird Client for Gnip Accounts

[intro]

https://github.com/twitter/hbc

(what is the Hosebird Client and why it is the Java way to go)

Features: reconnection logic with backoff, handles gzip compression


(HBC has been updated to support Gnip PowerTrack
    Basic Authentication
    Need: username password account product label




####Referencing the Hosebird Library Using Maven

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>sample-hbc-app</groupId>
    <artifactId>sample-hbc-app</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.twitter</groupId>
            <artifactId>hbc-core</artifactId> <!-- or hbc-twitter4j -->
            <version>2.2.0</version> <!-- or whatever the latest version is -->
        </dependency>
    </dependencies>

</project>
```


####Example Java Client


```
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

import com.twitter.hbc.ClientBuilder;
import com.twitter.hbc.core.Client;
import com.twitter.hbc.httpclient.auth.BasicAuth;
import com.twitter.hbc.core.Constants;
import com.twitter.hbc.core.endpoint.RealTimeEnterpriseStreamingEndpoint;
import com.twitter.hbc.core.processor.LineStringProcessor;

public class hbcClient {

    public static void main(String[] args) {
        try {
            //username, password, account, product, label
            hbcClient.run(args[0], args[1], args[2], args[3], args[4]);
        } catch (Exception e) {
            System.out.println(e);
        }
    }

    public static void run(String username, String password, String account, String product, String label) {

        /** Set up your blocking queues: Be sure to size these properly based on expected TPS of your stream */
        BlockingQueue<String> queue = new LinkedBlockingQueue<String>(100000);

        /** Declare the host you want to connect to, the endpoint, and Basic Authentication for Gnip streams */
        RealTimeEnterpriseStreamingEndpoint endpoint = new RealTimeEnterpriseStreamingEndpoint(account, product, label);
        BasicAuth auth = new BasicAuth(username, password);

        // NEW LineStringProcessor to handle Gnip formatted streaming HTTP
        LineStringProcessor processor = new LineStringProcessor(queue);

        // Build a hosebird client just like before
        ClientBuilder builder = new ClientBuilder()
                .name("PowerTrackClient-01")
                .hosts(Constants.ENTERPRISE_STREAM_HOST)
                .authentication(auth)
                .endpoint(endpoint)
                .processor(processor) ;

        Client client = builder.build();                   // optional: use this if you want to process client events

        // Attempts to establish a connection.
        client.connect();

        while (!client.isDone()) {
            try {
                String message = queue.take();
                System.out.println(message); // Here is where you could put it on a queue for another thread to come in and take care of the message
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}
```

####Running Client


http://maven.apache.org/download.cgi


```
me$ java hbcClient "username" "password" "account" "product" "label"

```

####Writing data to a Client database.
package com.twitter.data.client


import com.twitter.hbc.datastore.db

class database

end



