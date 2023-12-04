# Introduction
All of Amazon's database services are fully managed (don't need to worry about software updates,
etc), and also resilient.

# RDS
RDS is scalable, and some databases also allow for the creation of read-only replicas, that help
with increasing throughput on read-heavy workloads.

# DynamoDB
Key-value document database, with single-digit millisecond performance.

There is a primary key that is split into two parts: a partition key (numerical), and a sort key
(category/string etc).

Each key is related to several attributes. These attributes don't have to be the same across the
database, unlike a SQL database where all rows have to conform to a given schema.

Online storefront example:
- Primary key is a partition key labelled "Product ID", which is numerical, and a sort key that is
  labelled "Type". "Type" could be "Book", "Music Album", "Song", "Movie", etc
- The attributes can be different for each "Type":
  - Title of the book/album/song/movie
  - Author of the book
  - Artist(s) of the album/song
  - Director/producer/scriptwriter... of the movie etc

# Amazon ElastiCache
To improve access to database queries, cache the responses in memory.

Two different engines for ElastiCache: Redis and Memcached.
