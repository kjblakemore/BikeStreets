## Bike Streets Trips Data Management

This respository contains an architecture for management of [Bike Streets](https://www.bikestreets.com/) trip data.

The trip data is used by Analytics to answer the following questions:

1. How many people are riding on Bike Streets on a daily basis? (Note this is on the actual Bike Streets routes -- not just any old street.)
2. How popular is each segment of the Bike Streets routes? (I.e., heatmap.)
3. Where are people avoiding Bike Streets routes and preferring other workarounds? (So we can make adjustments to the map based on desire lines.)

The architecture diagram is:
![screenshot](architecture.jpg)

In this diagram, mobile devices upload trip data each day to the MongoDB server. Each bike trip is represented as a sequence of geo-coordinates and a flag indicating if this is the rider's first ride on Bike Streets for the day. An example trip data set can be found in **data/trips_03052020.json**.

The Aggregator program, then aggregates daily trip data from multiple mobile devices into two data sets.

1. **segment_freq.json** - Each entry in this data set consists of a coordinate pair, defining a *primary* segment and a corresponding frequency count. A *primary* segment is one with no intermediate intersections.  The Aggregator program accesses the Map Server to break *compound* segments into *primary* ones.
2. **rider_count.json** - Contains a count of Bike Streets riders for each day.

Analytics plots the **segment_freq.json** data set to answer questions two and three.  Question one is answered using the **rider_count.json** data set.