# Apache Creadur Website

This project contains the project's webpage.

Its deployment is done via [asf.yaml](./.asf.yaml). Every commit is directly pushed and available on https://creadur.apache.org/

## How to generate new page versions for RAT

## SNAPSHOT


### RAT
In order to sync the current webpage (taken from current SNAPSHOT) you need to run the following commands:

```
$ cd creadur-rat
$ mvn clean site:site site:stage
$ cd ../creadur-site
$ cp -rvf ../creadur-rat/target/staging/* ./rat/

Make sure to manually adapt download pages as they need to reference the current release and SNAPSHOT versions!
$ git commit -am "Update site build for RAT"
```

*WARNING!* This will sync the current master branch/SNAPSHOT version of the homepage if not run on the release branch

### Tentacles

Same, but:
```
$ cd creadur-tentacles
$ mvn clean site:site site:stage
$ cd ../creadur-site
$ cp -rvf ../creadur-tentacles/target/staging/* ./tentacles/

Make sure to manually adapt download pages as they need to reference the current release and SNAPSHOT versions!
$ git commit -am "Update site build for TENTACLES"
```

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

Make sure to manually adapt download pages as they need to reference the current release and SNAPSHOT versions!

$ git commit -am "Push new preview version of RAT 0.14"
```

This will allow a preview of the release site build at [rat014](./rat014)

### Release Notes / RAT-306

Make sure that https://creadur.apache.org/rat/RELEASE_NOTES.txt does not yield a 404.
RAT's changelog consists of 2 versions:
* [generated changelog of the current release](https://github.com/apache/creadur-rat/blob/master/RELEASE-NOTES.txt)
* [complete/historical changelog](https://github.com/apache/creadur-rat/blob/master/RELEASE_NOTES.txt)

Usually it helps to copy over the release-generated version into the subfolder /rat of this project

### Combined release notes

Make sure to generate a combined changelog that includes the most current release, available under [relase-notes/rat.txt](/release-notes/rat.txt)
