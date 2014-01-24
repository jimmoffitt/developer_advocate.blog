**Running Ruby/Tk Applications as Windows 7 Executables**

**Cross-Platform Goals**

A fundamental design details was that I wanted to develop a Ruby application on MacOS and end up with a tool our Windows users could use to help them manage the files produced by our Historical PowerTrack product.

**Three Fundamental Processes**
+ HTTP downloads of potentially 100's of thousands files.
+ Convert social media activities from JSON to CSV.
+ Consolidate 10-minute time-series files into hourly or daily files.  


**Why Tk?**
require 'tk'
require 'tkextlib/tile'




