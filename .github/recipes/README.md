# Scan recipes

In this directory you will find a few recipes to cover basic use cases for the scan workflow and action.
Those recipes are just generic examples on how to use the scan tools, and can (should) be mixed together to fit your requirements.

## After Build Scan
The [After Build Scan](./after-build-scan.yml) triggers a scan on the filesystem followed by a scan on the generated docker image.

By default (i.e. without inputs), the scan action uses `fs` mode on the repository root directory.
You need to overwrite these inputs in you need to scan images.

## Daily Automatic Scan
The [Daily Automatic Scan](./daily-automatic-scan.yml) triggers a scan on a regular basis.
It uses the more complete workflow that automatically determine the branch name and associated docker image using YYYYmm format used for releases in the WP.

The example does not show specificities of repositories that use an 'unconventional' naming.

For instance, if your release branches are names 202401-release, then you should use the `branch-regex` input with value `^%(date)-release$` regular expression.
The `%(date)` should be hardcoded explicitely in the regex as it will be replaced in the workflow by the actual date.

If your release images are named `202401-latest` then you should use the input `image-template` with value `$(date)-latest`.
Note that this is not a regular expression, and should therefore reflect exactly the names of your images, with the `%(date)` being always present as well.

The scan workflow will by default look for library CVEs in the filesystem.
In Java projects for instance, this does nothing, as the library CVEs have to be searched for in the docker image itself.
In this case, you should provide the input `scan-library-in-image` with value `true`.

## Daily Specific Scan
The [Daily Specific Scan](./daily-specific-scan.yml) triggers a scan on specified images and refs.

Refs are only useful if you want to update Github's security tab, if you don't the matrix could be a simple array of strings.

In this example the `scan-type` is set to `os,library` for simplicity, but note that the slack alert will then mix OS and library vulnerabilities into a single file.
To have a more comprehensive separation, two steps can be used to separate the two, like it is done in the [After Build Scan](./after-build-scan.yml).

## Ref Scan
The [Ref Scan](./ref-scan.yml) shows how to trigger a scan on a particular scan, defaulting to the most recent tag.
