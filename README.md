# TeX Live Core

[TeX Live] is a distribution of the TeX document production system.

[TeX Live]: https://www.tug.org/texlive/

This repository is a clone of core parts (configuration and scripts) of the TeX
Live Subversion [repository]. TeXbrew uses it for a [Homebrew] formula.

[repository]: https://www.tug.org/texlive/svn/
[Homebrew]: https://brew.sh/

## Updating this repository

We update this repository every time a TeX Live release is made or when some of
the included files change in a significant way.

* [`Build/source/texk/texlive/texlive`](https://www.tug.org/svn/texlive/trunk/Build/source/texk/texlive/?sortby=date&view=log)
* [`Build/source/texk/texlive/tl_scripts`](https://www.tug.org/svn/texlive/trunk/Build/source/texk/texlive/tl_scripts/?sortby=date&view=log)
* [`Master/texmf-dist/fonts/map/dvips/tetex`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/fonts/map/dvips/tetex?sortby=date&view=log)
* [`Master/texmf-dist/texconfig`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/texconfig?sortby=date&view=log)
* [`Master/texmf-dist/scripts/texlive/mktexlsr.pl`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/scripts/texlive/mktexlsr.pl?sortby=date&view=log)
* [`Master/texmf-dist/doc/info/tds.info`](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/doc/info/tds.info?sortby=date&view=log)

### Prepare for a release

For a new release, we need the following:

1. Subversion revision
2. Version number

We try to use the same Subversion revision for all of the above, though we may
need to treat them differently depending on the changes (e.g. by patching).

To find the revision, look at the history in the links above for changes
indicating a new release or a significant change. For example, [revision 46759]
indicates the imminent release of TeX Live 2018.

[revision 46759]: https://www.tug.org/svn/texlive?view=revision&sortby=date&revision=46759

We choose the version number using the date of the revision (or the last date if
multiple revisions are used). In the above example, the version is `2018.02.27`.

Update the variables in the [`version`](./version) file with the values found.

### Update the files

Run [`script/1-update-and-add`](./script/1-update-and-add) and verify the staged
differences shown.

If needed, you may need to make changes to the script, reset the files, and run
the script again.

### Update the local and remote repositories

Run [`script/2-commit`](./script/2-commit) and verify the new commit.

Run [`script/3-tag-and-push`](./script/3-tag-and-push) to create a tag, delete
any existing duplicate tag, and push to the remote repository.

### Test the remote release

Run [`script/4-test-release`](./script/4-test-release), which downloads the
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
