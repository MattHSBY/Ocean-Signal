All software tools used in production or externally must have controlled release procedures. The fundamental principles are:

- No two builds that differ in functionality should have the same version number
- All release software should be released from a build server, using an automated build
- Unit tests should be used where possible and practicable as evidence that the build is "good"
- Builds should be automated so that the entire build-test-deploy chain can be run with a single click or command.
There are very good reasons for each of these principles.

## Different Builds Must Have Different Version Numbers

It should be obvious why this is a good idea.

There are some demands placed on version numbers by the company quality system, i.e. controlled items are required to have a 2-part version number, `XX.YY` where `XX` is the major version and `YY` is the minor version. This is not flexible enough for many situations and so we have to adopt a system where we can have longer internal and pre-release version numbers while releases follow the company quality standards.

The recommendation is to adopt [Semantic Versioning 2.0][SemVer2]

[SemVer2]: 