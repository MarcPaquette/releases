## releases

List of releases tracked by https://bosh.io.

PR changes to index.yml to submit additional releases for review.

Checklist for submission:

- LICENSE and NOTICE files are up to date
- at least one final release is checked in on the default repo branch
- of use to the general community
- will be maintained
- github repo must be public
- `bosh create-release` must run successfully against any final releases
- a README explaining what the repo does
- [optional] Github repo has a descriptive project title
- [optional] manifests/ directory contains example manifest

 The bosh-io pipeline periodically runs a `bosh create-release ` 
 on any bosh releases above the minimum version specified to check for new versions.
 
 If any errors occur, we may create an Issue on the repo in question.
 If the release looks to have no recent activity we'll remove it from the pipeline. It would still be listed on bosh-io but any updates to the release would require another PR to this repo.
 
 *Note for maintainers: After merging a new release, remember to run the [worker](https://github.com/bosh-io/worker) sync scripts to configure the pipelines*


## FAQ

### How do I get a new release on bosh.io/releases?

First, make sure you can create a final BOSH release. Note that this is different than making a dev release.
See [docs](https://bosh.io/docs/create-release/#final-release) on bosh.io for more info.

Next, Submit a PR to add your release to [`index.yml`](https://github.com/bosh-io/releases/blob/master/index.yml).

Without specifying a `min_version`, bosh-io will pick up every release version, including old and possibly broken releases.
If you only want releases after a specific version, add a `min_version` to the release configuration in `index.yml`.

### How does bosh.io find my release?

bosh.io does not pay attention to GitHub releases.
It watches the `releases` directory within your GitHub repository on the default branch, which contains the metadata for final releases.
See the [cloudfoundry/bosh releases directory](https://github.com/cloudfoundry/bosh/tree/master/releases).
Note that the `releases` directory is automatically updated when creating a final release, you should not need to manually update `releases`.

### What is the .final_builds directory, do I need it?

Yes, you need it.
Creating a final release will update both the `releases` directory and the `.final_builds` directory.
The `releases` directory contains metadata about what goes into the release for each version.
The `.final_builds` directory has references to blobs for all the assets associated with a release: job templates, packaging scripts, etc.
Without the `.final_builds` directory, your release will not be added to bosh.io.
Make sure to commit `.final_builds` to Github.

### Why isn't the latest version of my release on bosh.io/releases?

Check the default branch of your GitHub repository and make sure the release is listed in the releases directory.
Without that file, bosh.io won't be able to import the release.

Make sure you cut a final release, not a dev release.

Ensure that running `bosh create-release releases/<release-name>-<version>.yml` from your github repo succeeds.

If everything is working in your release, it is possible there has been an internal Concourse failure.
Reach out to the BOSH team on [slack](https://cloudfoundry.slack.com/archives/C02HPPYQ2) or [github](https://github.com/bosh-io/releases/issues) to check the Concourse builds.

### I accidentally cut a bad release, how do I get it removed from bosh.io/releases?

Generally, existing releases listed on bosh.io will not be removed.
If you'd like to discourage use of an existing release, cut a new release with a higher minor version.
In extreme cases, feel free to reach out to the BOSH team.

## Additional Documentation

* [Create a Release](https://bosh.io/docs/create-release/)
* [Concourse resource for BOSH releases](https://github.com/concourse/bosh-io-release-resource)
