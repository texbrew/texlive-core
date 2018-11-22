# TeX Live Core

[TeX Live] is a distribution of the TeX document production system.

[TeX Live]: https://www.tug.org/texlive/

This repository is a clone of core parts of the TeX Live Subversion
[repository]. In particular, it contains `kpathsea` (a library for path
searching), various scripts, and configuration files.

TeXbrew uses the release archive in a [Homebrew] formula.

[repository]: https://www.tug.org/texlive/svn/
[Homebrew]: https://brew.sh/

## Updating this repository

We update this repository when a `kpathsea` release is made, when a TeX Live
release is made, or when some of the included files change in a significant way.

* [`Build/source/texk/kpathsea/`](https://www.tug.org/svn/texlive/trunk/Build/source/texk/kpathsea/?sortby=date&view=log)
* [`Build/source/texk/texlive/texlive/`](https://www.tug.org/svn/texlive/trunk/Build/source/texk/texlive/?sortby=date&view=log)
* [`Build/source/texk/texlive/tl_scripts/`](https://www.tug.org/svn/texlive/trunk/Build/source/texk/texlive/tl_scripts/?sortby=date&view=log)
* [`Master/texmf-dist/fonts/map/dvips/tetex/`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/fonts/map/dvips/tetex?sortby=date&view=log)
* [`Master/texmf-dist/texconfig/`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/texconfig?sortby=date&view=log)
* [`Master/texmf-dist/scripts/texlive/mktexlsr.pl`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/scripts/texlive/mktexlsr.pl?sortby=date&view=log)
* [`Master/texmf-dist/doc/info/tds.info`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/doc/info/tds.info?sortby=date&view=log)

### Prepare for a release

For a new release, we need the following:

1. Subversion revision number
2. Version

We use the **Subversion revision number** for the `svn checkout` of all files.
We may need to use additional revisions to create patches on top of the initial
checkout.

To find the revision number, look at the history in the links above for changes
indicating a new release or a significant change. For example, [revision 46759]
indicates the imminent release of TeX Live 2018.

[revision 46759]: https://www.tug.org/svn/texlive?view=revision&sortby=date&revision=46759

We choose the **version** using the year of the TeX Live release as the major
version number and a two-digit minor version number to indicate any changes
between annual TeX Live releases.

While developing and testing a given version, indicate that a release is a
**pre-release** by appending `pre` to the version. Pre-release tags are unstable
and may be added or removed arbitrarily. Once testing is complete, remove the
`pre` suffix.

### Update the files

First, **update the variables** in [`version`](./version) with values as
described above.

1. If you are starting on a new release, append `pre` to the version. Remove the
   `pre` suffix after you have tested the release archive.
2. Do not re-use an existing version unless it is a pre-release version.

Then, run [`script/1-update`](./script/1-update) to **update the files** of this
repository using `svn checkout` and `svn export` from the TeX Live repository.

If needed, make changes to the script or the files and run the script again.

When you are happy, **add the files** to the index (`git add`) and **commit
them** to the local repository with a useful message (`git commit`).

### Tag the commit and push to the remote repository

Run [`script/2-tag-and-push`](./script/2-tag-and-push) to create a tag, delete
any existing duplicate tag, and push to the remote repository.

### Test the remote release archive

Run [`script/3-test-release`](./script/3-test-release), which downloads the
release archive for the [`version`](./version) and attempts to configure and
build it. You can also browse the release archive to verify that no unnecessary
files are included.

## Excluding files

The purpose of this repository is for making release archives used by a Homebrew
formula. Consequently, we exclude some files from accidentally being used.

### From commits

We use [`.gitignore`](./.gitignore) to exclude files from being committed to the
repository. In particular, we want to ignore Subversion files such as the `.svn`
directories.

### From releases

We use [`.gitattributes`](./.gitattributes) to exclude all non-essential files
from a release archive (as created by `git archive`). Only `kpathsea`-related
files should be included.
