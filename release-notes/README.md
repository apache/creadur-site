# Release Notes Readme

The main webpage links to
https://creadur.apache.org/release-notes/rat.txt
in order to show a combined release notes document over *all* available versions.

Each dedicated version can be found in this subfolder and should be linked directly (if needed).

## rat.txt

```rat.txt``` and ```RELEASE_NOTES.txt``` should be kept in sync as they refer to the same release
and provide a combined release notes over **all** versions.

## RELEASE-NOTES.txt / RELEASE-NOTES-xyz.txt

are release-specific notes that contain the contents of the release announcement mail
and are generated with the help of the maven-changes-plugin in RAT's repo.

```
$ ./mvnw changes:announcement-generate -Prelease-notes -Dchanges.version=0.17
```
