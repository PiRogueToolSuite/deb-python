# Python packages for PiRogue

## Introduction

The goal of this repository is to hold minimal, proof-of-concept-level packaging
for some Python packages that are required for the PiRogue. The idea is to avoid
depending on running `pip3 install` in the target environment, and to have a
better grip on components that aren't shipped in the Raspbian or Debian
archives.

Usually, creating a new package would mean targetting unstable, then backporting
the package to stable; here, the immediate goal is moving away from a pip-based
installation, so the target is bullseye directly.

Some packages already exist in Debian, but the dependencies expressed initially
(by hardcoding versions in `pip3 install` calls within `preinst` scripts)
might require higher versions.

To bootstrap each package, the `py2dsp` (PyPI to Debian Source Package) tool is
used and its output is massaged into something suitable. Some items are unlikely
to get immediate attention, but should be checked if someone wanted to submit
one of those packages for inclusion into Debian proper:

 - copyright files;
 - long description in control files;
 - testsuites are sometimes disabled because they seem buggy, or would require a
   longer list of packages (e.g. `mocket` for `geoip2`).


## Workflow

Each Python package is pulled with a top-level `package/` directory along with
the original tarball (e.g. `package_version.orig.tar.gz`); this makes it easy
to build an actual source package that can be turned into binary package(s) by
passing it to a “clean build” building tool, like `cowbuilder`. The target as of
April 2022 is the `bullseye` distribution (Debian 11).

Modifications within each package directory should really be about adding a
`debian/` directory.

In the end, the resulting binaries can be merged into the
[third party section](https://github.com/PiRogueToolSuite/ppa/tree/main/pirogue-3rd-party)
of the [PiRogue PPA repository](https://piroguetoolsuite.github.io/ppa/).

The PPA might contain Python binaries whose sources aren't tracked in this
repository: `numpy` in the required version is available in `experimental` but
backporting it to `bullseye` seems non-trivial; its dependencies suggest it
should work unmodified with `bullseye`'s `python3` interpreter (3.9), so the
binary packages might end up getting imported from the Debian archive, rather
than built from source.
