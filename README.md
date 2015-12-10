# Remarkable

Remarkable deploys Marklogic's extensions, transformations and pipelines.
It does it fast and reliability. 
It's [Roxy](https://github.com/marklogic/roxy) on steroid and with a brain. 

## Goals

* Deploy your project's extensions, transformations and pipelines
* Deploy fast
* Deploy only what has changed
* Works in a cluster environment (machines don't overwrite their neighbor)

## Non goals

* Configure databases (eg. `ml-config.xml`, `all.xml`...)

## Requirements

* Java 8
* Marklogic 8.0.4 (may work with older 8.0.x versions)
* Dependency : Spring's RestTemplate

## How it works

1. On the target server, a document contains the list of handled objects (extensions, transformations...)
1. Remarkable locks the remote document (avoid collision if multiple apps starts Remarkable at the same time)
1. Remarkable compares local and remote objects by name and using a hash for the content
1. New objects are uploaded in parallel using Marklogic's Rest API
1. Changed objects are uploaded (name, hash and date are stored on the server)
1. Unknown objects are deleted if cleaning is enabled
1. Unchanged local/remote objects are ignored
1. Remarkable remove its lock

The whole operations is a matter of seconds for a few extensions and transformations. Uploads are done in parallel.

## History

This project is not endorsed by Marklogic©®™.
We had a problem and were very bored by our slow feedback loop, so we decided to do something.
A few helper class later and this project was born.

## Todo

* [ ] Deploy transformations
* [ ] Deploy extensions
* [ ] Deploy pipelines and domains
* [ ] "Clean": Delete unhandled objects (delete old extensions...)
* [ ] "Failsafe": Experiment if a transaction can be used to rollback any changes in case of failure (PostgreSQL can rollback 
DDL statements)
* [ ] "InitOnUpdate": Don't try to update if not a pre-configured target database
* [ ] "Validate": ask server to eval a script before uploading it