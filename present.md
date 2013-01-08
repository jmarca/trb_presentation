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

# CAP theorem

* Consistency
* Availability
* Performance

Choose any two



# Flat files

* One file per detector per year
* Fine for single user
* Not fine for multiuser (race conditions, version problems, etc)
* Good for annual stats generation, imputation of missing data, etc
* Difficult to query

# CouchDB

* Document model, not schema based
* Chooses *A*vailability, *P*erformance
* "Eventually Consistent"

# Consistency isn't often important for traffic data

Data is observed and written.

That observation doesn't change

#
