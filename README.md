# Apache Creadur Website

This project contains the project's webpage.

Its deployment is done via [asf.yaml](./.asf.yaml). Every commit is directly pushed and available on https://creadur.apache.org/

## How to generate new page versions for RAT

In order to sync the current webpage (taken from current SNAPSHOT) you need to run the following commands:

```
$ cd creadur-rat
$ mvn clean site:site site:stage
$ cd ../creadur-site
$ cp -rvf ../creadur-rat/target/staging/* ./rat/

```

*WARNING!* This will sync the current master branch/SNAPSHOT version of the homepage if not run on the release branch
