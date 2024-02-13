In this directory you will find a few recipes to cover basic use cases for the scan workflow and action.

The [After Build Scan](./after-build-scan.yml) triggers a scan on the filesystem followed by a scan on the generated docker image.

The [Daily Automatic Scan](./daily-automatic-scan.yml) triggers a scan on a regular basis.
It uses the more complete workflow that automatically determine the branch name and associated docker image using YYYYmm format used for releases in the WP.

The example does not show specificities of repositories that use an 'unconventional' naming.

For instance, if your release branches are names 202401-release, then you should use the `branch-regex` input with value `^%(date)-release$` regular expression.
The `%(date)` should be hardcoded explicitely in the regex as it will be replaced in the workflow by the actual date.

If your release images are named `202401-latest` then you should use the input `image-template` with value `$(date)-latest`.
Note that this is not a regular expression, and should therefore reflect exactly the names of your images, with the `%(date)` being always present as well.

