# Finding Change in the CouchDB

Pun intended.

Sorry

This project stated with the idea that PostgreSQL was fine for
everything I could throw at it.

then I downloaded raw 30s data for California from every VDS detector,
via the excellent PeMS data repository.

Routines that  worked for just Orange County, CA, fell down hard for
all of California.


At the exact same time...

I happened across a node on the Sakai mailing list about a  document
oriented database called CouchDB.

That went in one eyeball and was stashed for future processing.

After reading a bit, it seemed that CouchDB might be useful, but there
were lots of other databases that might also be useful for

lots of data

fast retrieval

flat, non-relational data storage.

tokyo cabinet was an early winner!

CouchDB worked, but had limits.

I couldn't figure out how to get Hadoop up and running quickly

Cassandra was very interesting, but not better than Tokyo Cabinet

And Tokyo Cabinet kept crashing!

So CouchDB won.



# Benefits

Easy replication

No really, *Super duper easy replication*

I can copy a database to my laptop with no issues.

Continuous or one-time, filtered or everything

So,

I have a nothion that every detector on the side of the road can be
running CouchDB and everybody can ask for filtered replication....I
just want data from some time of day, som eday of week, etc.


# What we did

## flat files

We used flat files, one per year per detector, because we were
processing on a year boundary.

We never needed any finer grained details on the raw data... just what
detector, what year.

Flat files are the fastest way to do that.

## CouchDB

But When I did need to access details, perhaps the daily profile from
a particular day at a particular detector, then CouchDB was superb.

Unlike PostgreSQL, the index is extremely explicit, defined as a
view. The downside is that CouchDB is *not* good at general purpose
searching and queries.  You can't efficiently explore the data and
test out different hypotheses.

But that is okay.

When you need really fast responses, then you are not doing
exploration work, but rather serving clients in some way (web, etc).
In those cases, clients and customers do not know enough to do
exploratory work anyway; they just want canned search results.

We manually sharded the data to speed access, processing
\footnote{more on that later}.

One database per year per detector.

One aggregate database per year per freeway (each day collapsed into
summary stats)

# november

A presentation about data storage.

*Why* is important, gives the application we wanted to achieve.

*What* is important.  Gives the scale and scope of the data being
considered.

Advantages of couchdb

1. allows easy replication
2. if you know what you want, you can get it fast
3. handles writes and reads
4. eventually consistent, but writes are time-based...don't change
past writes.  Therefore, best handling of CAP theorem for our case

Disadvantages of CouchDB

1. not great for exploratory analysis (views take a long time to
generate)
2. Sharding is manual, not out of the box.  BigCouch (Cloudant) and
coming soon to couchdb

Application:

Daily/Annual.

Model outputs 30s data, based on raw 30s observations.

Need raw data, need model output, need to be able to view annual
trends.

Application is to view a day's data, and view annual trends



# Presentation outline

1. Application generalities
2. Data sources
3. Full stop fail with postgresql.
4. Because the index was too fucking big to fit in RAM
5. We could have tuned it, but what the fuck
6. message on the Sakai mailing list "hey have you seen this document
oriented database called couchdb?"
7. What's a document oriented database
8. Document orientation
9. Column orientation
10.  No Schema!
11. No Indexing problem!
12. Indexing problems with views!
13. Erlang is rock solid awesome
14. What the fuck is Erlang?
15. Concurrent
16. Competitors failed to deploy.  Yardstick was me
   * Cassandra
   * Tokyo Cabinet/Tokyo Tyrant
   * Hadoop
   * Voldemort (didn't try it)
   * Riak (didn't exist)
17. Big early barrier was DB size, slowing down index generation
18. Figured out that we could just shard manually, speed up index
generation
19. Can't make views of views
20. big deal, sort of, but solvable with databases that are views of
views
21. Raw and processed db
22. Daily db
23. Raw and processed view generates daily summaries
24. daily summaries are copied to daily db
25. Web site uses both

26. Demo, run from laptop.

# Issues is to get the presentation to run from laptop

Record a session working from the laptop

Publish the URL







































