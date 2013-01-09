# Finding Change in the CouchDB

# Big Data

# We've got the biggest Data of them all

# New tools:  non-relational databases

(aka "NoSQL")

# Old tool:  PostgreSQL

# The question:

"Is the current reading of volume and occupancy from some detector
more or less than what should be expected?"

# Old way

Precompute some average value by time of day, and compare.

# But we had the data in  PostgreSQL, so...

# Big Query

"What is the average volume and occupancy for every 5 minute period
over the prior 365 days (less holidays) for the past year, for any
loop detector"

# Slow

The index was too big to fit in RAM, leading to heavy swapping

# That broke my database

# Big Data, NoSQL

* Google (Big Table)
* Amazon (Dynamo)

# Options considered

* flat files, CouchDB, TokyoTyrant, Cassandra, Hadoop

# Options available now

* Hadoop
* Riak
* MongoDB
* CouchDB
* Cassandra
* RethinkDB
* Voldemort
* Datalog

# Why do I think CouchDB is good for transportation data?

# Transportation data

* largely observations and measurements
* write once
* read raw data for short term applications
* process raw data into spatio-temporal summary stats

# Loop Detector

* 30s volume, occupancy
* some misses and noise
* might need known, fixed cleanup procedures to be applied

# Personal GPS and activity stream

* second by second GPS
* slowly growing sets of routes,  destinations, time windows
* some known queries, such as most likely next route for a traffic
  alert app

# Why do I think CouchDB is good for transportation data?

# CouchDB


# CAP theorem

* Consistency
* Availability
* Performance

Choose any two


# Compare with flat files

* One file per loop detector per year
* Append to file as data arrives
* Easy to organize and distribute
* Good for data cleaning, stats generation, imputation of missing
  data, etc
* Fine for single user
   * Consistent
   * Available
   * not performant

# Is performant a word?


# Problems with flat files

* Not fine for multiuser (race conditions, version problems, etc)
* Difficult to query
* "What is the 52 week running average of volume and occupancy for
   detector 120010 from 8:00AM to 8:05AM on Tuesdays?"


# CouchDB

* Document model, not schema based
* Chooses *A*vailability, *P*erformance
* "Eventually Consistent"

# Consistency isn't that big a deal for traffic data

* Events are observed by one or more sensors
* Sensors write their observations
* The *observations* don't change
* The interpretation of the *event* might change
* but a consistent interpretation isn't mission critical

# All replicating databases will be eventually consistent

# Relax

# Practical experience

(what we are doing with CouchDB)

# Storing raw plus stats for Orange County, California (D12)

* *fixme* 500 detectors
* *fixme* 200 mainline detectors
* Process in R
    * compute 27 different measures per location
    * for 20 minute running time window
    * (vol, occ per lane + 27 ) per 30s period
    * run models estimating relative risk of accident types

# Database design:

* One CouchDB database per detector
* One document per time stamp

# Document per time stamp reasons:

* Based on experience
* CouchDB sorts by document id, id is based on timestamp
* Typical query is to get a picture of trends over the day
* alternative is one document per day, but
    * messier to implement

# Database per detector reasons:

* One big database is possible, but
* Prior to BigCouch, impossible to *split* or *shard* over different
  machines
* makes better use of multi-core machines when generating views
* leverages "append-only" file structure (recent adds are faster to read)

# A view to compute daily summaries


# Application 2: Storing the results of imputation runs

# Application 3: Convenient stash for detector information
