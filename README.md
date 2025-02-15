Apache Solr Search
======================

This module integrates Backdrop with the Apache Solr search platform. Solr search can be used as a replacement for core content search and boasts both extra features and better performance.

Installation
------------

Prerequisite: Java 5 or higher (a.k.a. 1.5.x).  PHP 5.2.4 or higher.

Install the Apache Solr Backdrop module as you would any Backdrop module. Note
that the Backdrop branch does not require the SolrPhpClient to
be installed. All necessary code is now included with this module.

Before enabling the module, you must have a working Solr server, or be
subscribed to a service like Acquia Search.

The Debian/Ubuntu packages for Solr should NOT be used to install Solr.
For example, do NOT install the solr or solr-jetty packages.

Download the latest Solr 1.4.x or 3.x release (e.g. 1.4.1 or 3.6.1) from:
http://www.apache.org/dyn/closer.cgi/lucene/solr/

Apache Lucene 3.1, 3.2 or 3.3, have a possible index corruption bug on
server crash or power loss (LUCENE-3418) and have bugs that interfere
with the Backdrop admin reports. Solr 3.4 has a problem with
SortMissingLast so Solr 3.5.0 or later is strongly preferred.

Unpack the Solr tarball somewhere not visible to the web (not in your
webserver docroot and not inside of your Backdrop directory).

The Solr download comes with an example application that you can use for
testing, development, and even for smaller production sites. This
application is found at apache-solr-1.4.1/example.

You must use 3 Solr configuration files that come with the Backdrop
module or the integration will not work correctly.

For Solr 1.4 use the ones found in:
solr-conf/solr-1.4/

for Solr 3.5.0 or 3.6.1 use:
solr-conf/solr-3.x/

While the Solr 1.4 files will work for Solr 3.5+, they are not optimal
and you will be missing important new features.

For example, when deploying solr 1.4:

Move apache-solr-1.4.1/example/solr/conf/schema.xml and rename it to
something like schema.bak. Then move the solr-conf/solr-1.4/schema.xml
that comes with this Backdrop module to take its place.

Similarly, move apache-solr-1.4.1/example/solr/conf/solrconfig.xml and rename
it like solrconfig.bak. Then move the solr-conf/solr-1.4/solrconfig.xml
that comes with this module to take its place.

Finally, move apache-solr-1.4.1/example/solr/conf/protwords.txt and rename it
protwords.bak. Then move the solr-conf/solr-1.4/protwords.txt that comes
with this module to take its place.

Make sure that the conf directory includes the following files - the Solr core
may not load if you don't have at least an empty file present:
solrconfig.xml
schema.xml
elevate.xml
mapping-ISOLatin1Accent.txt
protwords.txt
stopwords.txt
synonyms.txt

Now start the solr application by opening a shell, changing directory to
apache-solr-1.4.1/example, and executing the command java -jar start.jar

Test that your solr server is now available by visiting
http://localhost:8983/solr/admin/

Now, you should enable the "Apache Solr framework" and "Apache Solr search"
modules. Check that you can connect to Solr at ?q=admin/setting/apachesolr
Now run cron on your Backdrop site until your content is indexed. You
can monitor the index at ?q=admin/settings/apachesolr/index

The solrconfig.xml that comes with this modules defines auto-commit, so
it may take a few minutes between running cron and when the new content
is visible in search.

To use facets you should download facetapi https://backdropcms.org/project/facetapi
This module will allow you to define and set facets next to your search pages.
Once this module is enabled, enable blocks for facets first at
Administer > Site configuration > Apache Solr > Enabled filters
then position them as you like at Administer > Site building > Blocks.

Access Sub-module
------------
The included Apache Solr Access module integrates with the node access
system using node access grants. It does not (and can not) work
with modules using hook_node_access() to block viewing of nodes because
it's impossible to apply those dynamic filters to as-yet-unknown search
results to return the correct number per page.  This same restriction
applies to any module that does content searching or listing (e.g. Views).

Settings.php
------------
You can override environment settings using the following syntax in your
settings.php

$conf['apachesolr_environments']['my_env_id']['url'] = 'http://localhost:8983';

Configuration variables
-----------------------

The module provides some (hidden) variables that can be used to tweak its
behavior:

 - apachesolr_luke_limit: the limit (in terms of number of documents in the
   index) above which the module will not retrieve the number of terms per field
   when performing LUKE queries (for performance reasons).

 - apachesolr_tags_to_index: the list of HTML tags that the module will index
   (see apachesolr_index_add_tags_to_document()).

 - apachesolr_exclude_nodeapi_types: an array of node types each of which is
   an array of one or more module names, such as 'comment'.  Any type listed
   will have any listed modules' hook_node_update_index() implementation skipped
   when indexing. This can be useful for excluding comments or taxonomy links.

 - apachesolr_ping_timeout: the timeout (in seconds) after which the module will
   consider the Apache Solr server unavailable.

 - apachesolr_optimize_interval: the interval (in seconds) between automatic
   optimizations of the Apache Solr index. Set to 0 to disable.

 - apachesolr_cache_delay: the interval (in seconds) after an update after which
   the module will requery the Apache Solr for the index structure. Set it to
   your autocommit delay plus a few seconds.

 - apachesolr_query_class: the default query class to use.

 - apachesolr_index_comments_with_node: TRUE | FALSE. Whether to index comments
   along with each node.

 - apachesolr_cron_mass_limit: update or delete at most this many documents in
   each Solr request, such as when making {apachesolr_search_node} consistent
   with {node}.

 - apachesolr_index_user: Define with which user you want the index process to
   happen.

Issues
------

Bugs and feature requests should be reported in [the Issue Queue](https://github.com/backdrop-contrib/apachesolr/issues).

Current Maintainers
-------------------

- [Eli Lisseck](https://github.com/elisseck).
- [Anthony Nemirovsky](https://github.com/anemirovsky).

Credits
-------

- Backdrop development supported by [USENIX](https://www.usenix.org/).
- Backdrop development supported by [Giant Rabbit](https://giantrabbit.com).
- Ported to Backdrop CMS by [Alejandro Madrigal](https://github.com/alemadlei) & [Eli Lisseck](https://github.com/elisseck).

License
-------

This project is GPL v2 software.
See the LICENSE.txt file in this directory for complete text.
