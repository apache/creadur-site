# Apache Creadur Website

This repository contains our webpage.

Every commit is directly pushed and available on [https://creadur.apache.org/](https://creadur.apache.org/), as its deployment is done via [asf.yaml](./.asf.yaml).

## Prerequisites for working on the webpage

In order to work with Creadur's git repositories some scripts expect that all projects are checked out in one directory. Thus you should have the following directory structure on your local box:

```
$ cd ~/myworkspace
$ ls -l
drwxrwxr-x 16 user user 4096 Okt  9 19:37 creadur-rat
drwxrwxr-x 13 user user 4096 Okt  9 20:28 creadur-site
drwxrwxr-x 10 user user 4096 Okt  9 18:25 creadur-tentacles
drwxrwxr-x 16 user user 4096 Okt  9 17:17 creadur-whisker
```

## How to generate new page versions for Creadur subprojects

## RAT - SNAPSHOT

The current SNAPSHOT version of RAT can be found in [rat100](https://creadur.apache.org/rat100/) and is generated manually.

RAT uses [Jira](https://issues.apache.org/jira/secure/RapidBoard.jspa?rapidView=625&quickFilter=2851) to track issues of the currently planned release.

### Apache RAT
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

Have a look in [RAT's buildtools folder](https://github.com/apache/creadur-rat/tree/master/.buildtools) if you want to generate a preview webpage.

### Apache Tentacles

Same, but:
```
$ cd creadur-tentacles
$ mvn clean site:site site:stage
$ cd ../creadur-site
$ cp -rvf ../creadur-tentacles/target/staging/* ./tentacles/

Make sure to manually adapt download pages as they need to reference the current release and SNAPSHOT versions!
$ git commit -am "Update site build for TENTACLES"
```
Have a look in [TENTACLES' buildtools folder](https://github.com/apache/creadur-tentacles/tree/master/.buildtools) if you want to generate a preview webpage.

### Apache Whisker

Same, but:
```
$ cd creadur-whisker
$ mvn clean site:site site:stage
$ cd ../creadur-site
$ cp -rvf ../creadur-whisker/target/staging/* ./whisker/

Make sure to manually adapt download pages as they need to reference the current release and SNAPSHOT versions!
$ git commit -am "Update site build for WHISKER"
```
Have a look in [WHISKER's buildtools folder](https://github.com/apache/creadur-whisker/tree/master/.buildtools) if you want to generate a preview webpage.

## Release troubleshooting

### No site can be generated - build failure during javadoc generation

Due to problems when generating Javadoc do only use JDK16! Newer versions cannot generate the project's javadoc and run into an NPE during path traversal. This is documented in [RAT-497](https://issues.apache.org/jira/browse/RAT-497) und reported upstream as a JDK bug.

### Keeping release artifacts locally

It may happen that you need access to the original release candidate JARs in order to have all of the necessary test JARs to perform a site build,
see [RAT-341](https://issues.apache.org/jira/browse/RAT-341) for details.

As a workaround copy these files into your local .m2 repository as a deployment of an already released artifact is not possible. Run a site build afterwards:
```
$ mvn site:site site:stage
```

### Building the webpage for releases/release candidates

Generate the webpage from the release tag, e.g. RAT 0.17:

```
$ cd creadur-rat
$ git checkout apache-rat-project-0.17
$ .buildtools/generateStagingSiteInWebpageRepo
OR
$ mvn site:site site:stage
```

Verify contents under target/staging:

Make sure to manually verify download pages as they need to reference the current release and future SNAPSHOT versions!

```
$ git commit -am "Push new preview version of RAT 0.17"
```

### RAT-306: Fix errors in release notes

Make sure that https://creadur.apache.org/rat/RELEASE_NOTES.txt does not yield a 404.

RAT's changelog consists of 2 versions:
* [generated changelog of the current release](https://github.com/apache/creadur-rat/blob/master/RELEASE-NOTES.txt)
* [complete/historical changelog](https://github.com/apache/creadur-rat/blob/master/RELEASE_NOTES.txt)

Usually it helps to copy over the release-generated version into the subfolder [./rat](/rat) of this project.

### Combined release notes

Make sure to generate a combined changelog that includes the most current release, available under [relase-notes/rat.txt](/release-notes/rat.txt)

## Local development with docker

The whole page can be started locally with the help of [docker-compose](https://docs.docker.com/compose/) and will provide the contents via http://localhost:8888
Depending on your docker version this means in the current repository:
```
$ docker-compose up
OR
$ docker compose up
```
You may need to configure shared paths from Docker -> Preferences... -> Resources -> File Sharing in order to mount your local files.

## Checking for broken links

In order to prepare release documentation you may want to start the webpage locally and issue:

```
$ linkchecker http://localhost:8888/rat
$ linkchecker http://localhost:8888/rat100
$ linkchecker --check-extern https://creadur.apache.org/rat100/
```

to look for broken links automatically.

If you do not have a local linkchecker installed, you may use docker as well:

```
docker run --rm -it -u $(id -u):$(id -g) -v "$PWD":/mnt ghcr.io/linkchecker/linkchecker:latest --verbose -F text/utf8/mnt/linkcheck.txt  index.html
```

Happy hacking :)
