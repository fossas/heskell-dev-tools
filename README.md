# haskell-dev-tools

This repo serves two purposes:

- Creating the docker image that is used to run the linter and formatter in
  the CI builds for the fossas/spectrometer repo.
- Automatically creating PRs for the image when the tools are updated.

Most of this repo is in the `.github/workflows` folder, but the two special
items are the Dockerfile, and the script that updates it: `update-dev-tools.sh`.

The image that is built is scoped to this repo as well as the org, but is made
publicly available, since there's no org-specific secrets or magic, and it
makes docker auth easier in spectrometer's CI. On that note, the code in this
repo is released warranty-free into the public domain, under
[The Unlicense](LICENSE).

The image is published as `ghcr.io/fossas/haskell-dev-tools:{version}`, where
`version` is the GHC compiler version we target.
See the [package page](https://ghcr.io/fossas/haskell-dev-tools) for more details.

## Updating the container

### How to update the version of a tool in the container

Don't.  This repo automatically updates tools to their latest versions, and
will override ANY version change in the dockerfile.

### How to add a new tool to the container

See #22 for an example PR.
You can enable auto-merge on your PR, and it will automatically publish to
the container registry after about 1 hour (due to long build times).

Once it's published, you can immediately use it in the CI at the
fossas/fossa-cli repo.

## Changelog

### GHC 9.8.2

- GHC is now version 9.8.2
- cabal-install is now 3.10.3.0

### GHC 9.4.8

- GHC is now version 9.4.8
- cabal-install is now version 3.10.2.0

### GHC 9.4.7

- cabal-install is now version 3.10
- **Breaking:** `prune-juice` has been removed. It is [deprecated](https://github.com/dfithian/prune-juice#readme).
  Users of this image should use `-Wunused-packages` [instead](https://downloads.haskell.org/~ghc/9.4.7/docs/users_guide/using-warnings.html?highlight=wunused%20packages#ghc-flag--Wunused-packages).
- **Breaking** `packdeps` has been removed.
  It has not had any updates since [2021](https://github.com/snoyberg/packdeps),
  requires building with GHC 8, and `cabal` now offers [similar functionality](https://cabal.readthedocs.io/en/stable/cabal-package.html#listing-outdated-dependency-version-bounds).
