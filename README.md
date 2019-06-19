## News / Changes
Please see CHANGES.md

## What is it
Started in 2003 as a simple flac to ogg vorbis script, flac2all has grown into a clustered parallel processing program that will convert your collection of FLAC files into various other formats (70+ formats if you meet all dependencies), complete with any tags that the source file had. Designed to be extended with new formats easily as time goes on, it is a utility for people with with large FLAC collections who also want a way to convert multiple files in parallel.

## Details

Version5 is the new release of flac2all. The decision to bump up a version number was primarily driven by the move to python3. Python2 is scheduled to be EOL after the 1st January 2020, and a lot of distros are already defaulting to python3 as the system Python interpreter. Rather than hold two versions of "version4", one for py2 and one for py3, it made more sense to bump the version number (thats what they are there for, after all).

Following on from my tradition of adding at least one major feature in major version upgrades. Version5 has the following new features:
* Support for ~72 new codecs via ffmpeg. Actual number of codecs subject to change based on what version of ffmpeg you have installed, and what options it is compiled with
* support for network distributed transcoding via ZeroMQ. This allows you to launch a single flac2all "master" on a machine, and then have flac2all "workers" running on other machines connect to it over a TCP connection. In other words, you can delegrate encoding tasks to multiple computers, each with multiple cores. For more details see the Usage->clustering section.

Note that the clustering function is optional, and flac2all will still work in the original way if ZeroMQ (and its python bindings) are not installed. As such we have not placed a hard dependency on ZeroMQ. This may change in future (e.g. if we decide to abandon the old logic and make everything ZeroMQ based internally).

## Dependencies
* Python >= 3.6
* Flac

## Optional dependencies
* ZeroMQ (for clustering)
* Lame: for mp3 support
* Opus-tools: for opus support
* Vorbis-tools: for ogg support
* aac-enc for AAC support
* ffmpeg for supporting all the audio encoders it supports

## Packages for Distros

### Stable:
There is a pip package available. You can install flac2all by running  "pip install flac2all" as root, or "pip install flac2all --user" for a non root local install.

To upgrade to a new release you run the same commands as installation, but with "--upgrade" option set.

## Development:
If you want the bleeding edge version, best to check out the latest "version5" branch from git.
Generally development work will be done in branches then merged, so master should be functional.

Tu run the version straight from the git repo, cd to "flac2all_pkg", and then run "python ./\_\_init\_\_.py -h". The rest should work as normal.

Since version 4, all codecs are split into their own modules, which allows developers to easily add new codecs. The internal function tables stay the same, meaning that as long as you follow the structure of the main functions, you can add any codec you want.

The easiest way to get started writing a codec module is to look at an existing one. I would recommend "flac.py", as it shows both encoding and decoding, and flac to flac conversion was very simple to implement. A more complex example is the mp3 module, which shows how complex things can get.

### Fixed branches
There are some branches that are considered "fixed". This means that they tend to be self contained, and they need not track any other branch. A list of these branches as as follows:

* master: Main branch, where final merges and tests are done prior to tagging and deployment. From here we generate the releases.
* version5: The current development branch, where changes are made, pulls merged and tested, prior to merge with master for release.
* version4: The current stable branch. No active development, but general maintenance and bugfixes are being handled.
* version3: The old stable branch. No active development, but kept in case someone needs/wants access to the old version3


### Dev etiquette
If you wish to contribute to flac2all, I ask that you keep to the following guidelines:

* If you want to extend/modify flac2all, checkout the latest copy of the repo, switch to the development branch, and then make your own branch, develop/test/debug until ready, then issue a pull request. Do not start hacking away on the master branch directly.

* The goal of the master branch is to be the final stage before a tagged release. As such, any code merged into master should not break things badly. All testing and debugging should be done on your branch prior to merge.

* If you want to add a new codec to flac2all, please keep it all in one single file. This is how the rest of flac2all is written. Some codecs (like AAC) actually have two implementations in one module (Both NeroAAC and fdk-aac). It keeps things easier to manage if each conversion codec is its own module.

* Keep to the same internal API as the other modules. If you strongly feel that the API is missing some functionality critical to making your module work, raise an issue on this project page and we can discuss the situation.

* Please raise an issue if you intend to place a hard dependency on a third party package (that isn't a codec). If you want to make use of a third party library it would be best to discuss before time is put into development.

## Known bugs/issues and TODOs

* using "ctrl-c" to terminate does not exit cleanly. Plus you have to hit ctrl-c multiple times to terminate flac2all.

* following on from above, when terminated the script leaves a bunch of tmpfiles. We need to clean up properly.

## Raising a bug report

Before you raise a bug report, please test with the latest version from git. Sometimes distro packages lag the latest stable by a couple of versions, so you may hit bugs that have already been fixed.

## Examples in use

* Here is a video of flac2all (v3) saturating a 16 core machine with conversions: [[synapse-16_threads](https://www.youtube.com/watch?v=pXSpPjWtSJc)]

## Usage

Full information, including options and all current available conversion modes, can be found by running "flac2all -h".

### Non clustered (original) ###

Please see USAGE.md

### Clustered ###

Please see USAGE-CLUSTERED.md

