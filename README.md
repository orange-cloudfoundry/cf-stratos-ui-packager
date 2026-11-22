# Cloudfoundry Stratos-UI Packager

## Description

This project is used to package the [Stratos-UI](https://github.com/cloudfoundry/stratos). 

## Usage

### Generating a New Release

Run `./bin/release` to start the update workflow on GitHub. It requires the GitHub CLI to be authenticated with permission to run workflows. The command returns after dispatching the workflow; it does not modify the local working tree. Inspect its progress with the `gh run list` command it prints.

### Automatic updates

The `.github/workflows/update-and-tag.yml` workflow runs daily and can also be started manually. It selects the most recently created Stratos version tag, including prereleases, updates the submodule on `master`, and creates the matching packager tag. An existing tag is never moved. If the matching release or its `stratos-ui.zip` asset is missing, it dispatches the release workflow again on the next run. The workflow needs Actions and repository contents write permissions, and `master` must allow the Actions token to push.

The workflows run their shell logic from `bin/`. The `.github/workflows/releaser.yml` workflow can also be triggered by a version tag push or manually. It checks out the latest workflow scripts from `master` separately from the tagged release source, allowing a failed older release to be retried with current scripts. It performs the following steps:

1. Checks out the repository and its recorded `stratos-ui` submodule commit.
2. Verifies that the workflow runs from a tag and that the submodule commit matches the Stratos tag with the same name.
3. Sets up Node.js, Bun, and Go using the versions specified by the checked-out Stratos submodule.
4. Builds and packages Stratos for Cloud Foundry with `make build cf` and `make release cf`.
5. Creates a new release with the packaged files.

The published `stratos-ui.zip` is a prebuilt Linux/amd64 Cloud Foundry package containing the backend binary, frontend assets, and deployment manifest. For a manual release run, select a tag or provide `release_tag` when running from a branch.