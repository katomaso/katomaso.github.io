# Semantic Release

You know what it is, right? So let's get into the gotchas. Here are faults that I encountered
while using `.github/workflow/my-workflow.yml` with following actions
```
jobs:
  release-version:
    name: Release new version
    runs-on: ubuntu-latest
    outputs:
      new_release_published: ${{ steps.semantic.outputs.new_release_published }}
      new_release_version: ${{ steps.semantic.outputs.new_release_version}}
    steps:
      - name: Checkout repository (full-depth)
        uses: actions/checkout@v3
        with: { fetch-depth: 0 } # Required to determine version
      - name: Semantic Release
        uses: cycjimmy/semantic-release-action@v3
        id: semantic   # Need an `id` for output variables
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

## gotcha-1: publish only from master
```
This test run was triggered on the branch DH-960_fwbundler_update, while semantic-release is
configured to only publish from master, therefore a new version won’t be published.
```
This actually means that the branch you are currently on was not matched by any of the 
[micromatch](https://github.com/micromatch/micromatch) expression that you write to "name"
of the branch.

## gotcha-2: invalid 
```
SemanticReleaseError: The pre-release branches are invalid in the `branches` configuration.
```
That might mean that you set `"prerelease": "some-static_value"` as I did and multiple branches
resolve to the same pre-release tag. That is unfortunately forbidden but this error message does
not help you to figure that out.
