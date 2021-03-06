# omim

Manual builds of the https://github.com/mapsme/omim/ based on official CI Linux image

The build triggers on tags and tags are to match `release-XXX` tags in https://github.com/mapsme/omim/
Example:
* source: release-95; this repo: release-95-base followed with release-95; DockerHub: release-95-base (first step) and release-95, release-95-full, latest, release-95-full-debug (skipped), release-95-generator, release-95-generator-debug (skipped) on second step
* source: release-96; this repo: release-96-base followed with release-96; DockerHub: release-96-base (first step) and release-96, release-96-full, latest, release-96-full-debug (skipped), release-96-generator, release-96-generator-debug (skipped) on second step

**IMPORTANT**: Due performance issues with DockerHub Automated Build these builds are separated!

The build process (Docker Hub Automated Build) two steps for a reason as outlined above. Unfortunately, it also caused the dirty hooks conditions as we need decide what step we are at. Input is very welcome!

First of all we need to prepare base image by creating tag `release-XXX-base`. In few hours (!) we get the DockerHub base image `alexkuzko/omim` with tag `release-XXX-base`.
It is possible to omit the creation of `release-XXX-base` tag and directly build and push it to save time by executing `build.sh` script from the root of this repository (this repository and Docker Hub Registry owner only).

Second step triggered by creating tag `release-XXX` to build DockerHub image `alexkuzko/omim`  with primary tag `release-XXX` (it also tagged as `release-XXX-full` and `latest`). Additionally we build images with tags `release-XXX-full-debug`, `release-XXX-generator` and `release-XXX-generator-debug`.
** IMPORTANT**: Same performance issue affects automated build, the script `build.sh` may be used to perform local build.

** DEBUG BUILDS: ** Due build issues with debig builds (they fail too often) they are no longer built/pushed automatically via `hooks/post_build / hooks/post_push` scripts unless argument passed. This means it may be built only using `build.sh` with additional second param passed.

Automated Build setup to trigger on:

Base image:
* Source Type: Tag
* Source: `/^(release-[0-9]+)-base$/`
* Docker Tag: `{\1}-base`
* Dockerfile location: Dockerfile
* Build Context: /
* Build Caching: Yes

Production images:
* Source Type: Tag
* Source: `/^(release-[0-9]+)$/`
* Docker Tag: `{\1}`
* Dockerfile location: Dockerfile.release
* Build Context: /
* Build Caching: Yes
