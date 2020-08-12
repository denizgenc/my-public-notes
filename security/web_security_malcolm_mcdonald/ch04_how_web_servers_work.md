# Chapter 4 - How web servers work
This chapter covers:
- Static resources
  - Content delivery networks (CDNs)
  - Content management systems (CMSes)
- Dynamic resources
  - Templates
  - Databases
  - Programming languages

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
