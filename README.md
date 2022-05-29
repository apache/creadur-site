# Apache Creadur Website

This project contains the project's webpage.

Its deployment is done via [asf.yaml](./.asf.yaml). Every commit is directly pushed and available on https://creadur.apache.org/

## How to generate new page versions for RAT

## SNAPSHOT

In order to sync the current webpage (taken from current SNAPSHOT) you need to run the following commands:

```
$ cd creadur-rat
$ mvn clean site:site site:stage
$ cd ../creadur-site
$ cp -rvf ../creadur-rat/target/staging/* ./rat/

```

*WARNING!* This will sync the current master branch/SNAPSHOT version of the homepage if not run on the release branch

## Releases

Due to problems when generating Javadoc do only use JDK16! Newer versions run into NPE during path traversal.

```
$ cd creadur-rat
$ git checkout apache-rat-project-0.14
$ mvn clean site:site site:stage

Verify contents under target/staging

$ cd ../creadur-site
$ mkdir rat014
$ cp -rvf ../creadur-rat/target/staging/* ./rat014/
```

This will allow a preview of the release site build at [rat014](./rat014)
