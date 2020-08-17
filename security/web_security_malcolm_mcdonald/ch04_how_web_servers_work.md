# Chapter 4 - How web servers work
This chapter covers:
- Static resources
  - Content delivery networks (CDNs)
  - Content management systems (CMSes)
- Dynamic resources
  - Templates
  - Databases
  - Programming languages
  - JavaScript frameworks

## Static and Dynamic Resources
Websites used to be made of handwritten HTML served direct from the filesystem. URLs would
correspond to the locations of the HTML files on the server's disks (hence the `.html` extensions in
the URLs of old websites). The file is then returned as is in an HTTP response.

### URL resolution
Modern servers retrieve static resources similarly to old ones: a URL has a resource name, and it
corresponds to a file on disk.

However, on modern servers, any URL can be mapped to any static resource, no matter the actual
location on the underlying filesystem. E.g. `/media/big_buck_bunny.mkv` can in be `/movies/bbb.mkv`.

Static resources can also be processed in some way before being sent to a client, or the headers on
the HTTP response might be edited. Common examples are:
  - Compressing the resource with `gzip`
  - Using caching headers to tell the client to cache the resource

Static resources are just files, so they don't inherently pose a threat to a server. However, the
URL resolution process can. Forexample, if a file is supposed to be private, access control has to
be implemented on the server. Attacks on access control are discussed in chapter 11.

### Content Delivery Networks
Instead of sending a file to directly from a server to a client, instead have a worldwide set of
data centres request the resource once and then serve to clients for you. Important for large files,
like images.

CDNs mean someone else is using your security certificate to serve content. This requires care, and
is covered in chapter 14.

### Content Management Systems
Most websites are mostly just static content (blogs, etc). Instead of building these sites from
scratch, use a content management system (CMS). Allows for people to create blog posts, upload
images, etc without writing HTML, as they provide authoring tools.

CMSes also have plug-ins that extend functionality (remove spam comments, analytics, creating
storefronts).

This is technically more secure since CMSes and their related plug-ins have an incentive to maintain
their code so they are free of vulnerabilities. However, this requires the CMS to be frequently
patched - so it's easy to find insecure websites. The risks around using other people's code is
discussed in chapter 14.

## Dynamic Resources
Authoring a new page (even using a CMS) can be time consuming for certain repetitive entries, e.g.
pages for products on a storefront. Instead, we can use dynamic resources.

These tend to populate an HTTP response with data from a database. Most of the time, dynamic
resources are HTML files, but they can be something else (e.g. a JSON response for an API, I
assume). This means you can have one "web page" that can represent multiple different things, like
different products in an online shop. To update the website, simply update the database.

Dynamic resources can lead to vulnerabilities. JavaScript injection is covered in chapter 7, and
HTTP requests from other websites is covered in chapter 8.

### Templates
Early dynamic resources were actually just Perl scripts. Perl is difficult to read by itself, but
trying to divine the layout of a page from a script is harder, since it requires you to reason not
just about the programming language, but also the HTML it outputs.

Because of this, we now have template files. These are mostly HTML, but have programmatic elements
that is interpreted by the server. These program blocks tend to do one of three things:
  - Interpolate data from database
  - Conditionally render the following HTML
  - Loop over a list of items to create multiple blocks of HTML.

### Databases
Need to store data somewhere. We have product IDs that have related data (e.g. price), user IDs with
related data (e.g. friends), we have usernames and passwords to store etc.

Two main types of databases. SQL and NoSQL.

#### SQL
A declarative programming language for *relational* databases. This means data is stored in tables,
where each row is an individual entry and each column is a particular property that is recorded for
entries in the table. Columns have predefined data types - strings, numbers, dates, etc.

Different tables in the same database refer to each other using keys. A table has a primary key
(usually a unique number) that identifies rows in the table, and there may be a foreign key column
in that table that represents primary keys (and hence other rows) from other tables.

There are data integrity constraints on relational databases, that prevent corruption of the data.
Constraints can be defined for a table - e.g. a `username` column being required to have unique
values, so no two users can have the same username.

SQL also has the concept of transactions and consistent behaviour. A transaction is a grouping of
SQL statements to be executed; if any one of those statements fails, the whole transaction fails,
and the database is left unchanged. SQL databases are consistent in that the whole database must
abide by its constraints - a transaction will never leave it in a state where it has invalid data.

Data in SQL databases tends to be rather sensitive, so is the target of hackers. Also, SQL injection
can cause issues, and is covered in chapter 6.

#### NoSQL Databases
SQL databases have some difficulties with performance, especially when multiple different queries
are being made. Because of these issues, NoSQL has become more popular. NoSQL databases don't have
the strict constraints of relational databases to be faster. There's lots of different types of
NoSQL databases, but there's some commonalities as well.

NoSQL databases tend not to have schemas, so every record can have different columns (data fields).
Schemaless data is often in key-value form, or in JSON format. This makes storing unstructured or
semi-structured data pretty easy.

NoSQL databases don't prioritise consistency as much. With SQL, a guarantee is that different
clients, making the same query at the same time, will see the same result. Most NoSQL databases will
instead aim for *eventual* consistency.

However, querying NoSQL databases can be harder than SQL ones - some may have an API for a
programming language, while others may use SQL-like syntax. Therefore it's more difficult for
hackers to mount an injection attack on a NoSQL database, but it's still possible.

### Distributed Caches
Distributed caches have data stored in memory, which makes retrieving data from them incredibly
quick. Examples like Redis and Memcached allow different software (and even different servers) to
store and share data (including data structures, which can be accessed in a language-agnostic way).

When a system is implemented in terms of microservices, the microservices communicate using
distributed caches. They can either use queues which are stored on the cache, or using
publish-subscribe channels.

The same sort of attacks on databases can also be performed on distributed caches. Redis and
Memcached provide SDKs that enforce best practices, so these can be avoided.

### Web Programming Languages
So many languages, so little time.

#### Ruby (on Rails)
- Invented in the mid-90s
- Became popular in the mid-00s with Ruby on Rails
  - Rails represents best practices for large web apps
  - Takes security seriously
- Smaller Ruby servers use microframeworks like Sinatra.
  - Extend capabilities of microframeworks using Ruby Gems package manager

#### Python
- Whitespace
- Popular with data science and scientific computing
- Variety of web frameworks/servers: Django, Flask, etc.

#### JavaScript and Node.js
- Node.js runtime is based on Google's V8 JS engine (from Chrome browser)
- Quite nice to be able to use same language on client and server
- Biggest security risks with Node is due to rapid growth - lots of packages, not enough auditing.

#### PHP
- Developed from C binaries to build dynamic sites on Linux
- Then grew into a fully-fledged programming language
  - Haphazard growth = haphazard language
- PHP usually found in legacy systems.
  - Modern libraries and frameworks are secure, but legacy systems tend not to use those... Make
    sure to update.

#### Java
- A programming language which runs in its own virtual machine - the Java Virtual Machine (JVM)
- Decent language where performance is a concern
- Not as popular for web dev as it used to be, but still powers a decent amount of its internet
- Legacy applications run on older, insecure versions of Java and the JVM. Make sure to update.
- Other languages use the JVM (therefore can use Java libraries):
  - Clojure (Lisp dialect)
  - Scala
  - Kotlin

#### C#
- A programming language which runs in its own virtual machine - the Common Language Runtime (CLR).
- Supports C++ code as well
- Reference implementation of C# is now open source
- Mono is an implementation of the CLR that allows .NET applications (such as C# apps) to run on
  Linux and other OSes.
  - However most companies that use C# will be deploying to Microsoft infra - Windows servers etc
    - This can be a cause for concern, security wise

#### Client-Side JavaScript
Despite all these choices available to developers on the server side of the web, there's only one
option when it comes to running code in the browser - JavaScript (though there's also languages
like TypeScript that are transpiled to JS).

JavaScript allows websites to be really dynamic now (redrawing whole sections of the page when the
user interacts with it, e.g. opening modal windows in Twitter; or notifying users when background
events happen). To do this without refreshing the page requires managing a lot of state in memory,
which is really complicated - so frameworks have been created to make this simpler.

One example of a JavaScript framework is Angular, by Google:
- Similar to server-side templating engines - Angular is a bunch of JavaScript that parses and
  renders templates *client-side (in the browser)*
  - Template HTML is provided by the server
  - Angular engine parses it and processes directives (for loops, etc)
  - Since this is JavaScript, it can directly update the DOM to update the page (this
    "short-circuits" some parts of the rendering pipeline).

Another example is React, by Facebook:
- Instead of writing templates, write HTML tags directly into JavaScript:
  ```jsx
  somefunc(someargs) {
    ...
    return <h1>Hello, {format(user)}</h1>
  }
  ```
- These are JavaScript XML files, usually with an extension `.jsx`, that are compiled into
  JavaScript before being served to a client

These frameworks enable complex, dynamic sites to be created, but because they directly manipulate
the DOM they open up a unique attack vector: DOM-based XSS. These are explored in chapter 7.
