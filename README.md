# Cloudfoundry Stratos-UI Packager

## Description

This project is used to package the [Stratos-UI](https://github.com/cloudfoundry/stratos). 

## Usage

### Generating a New Release

To generate a new release, you can use the `bin/release` script. This script will:

1. Reset the working directory to the root of the repository.
3. Update submodules.
4. Check for the latest tag in the `stratos-ui` submodule.
5. If a new version is found, it will check out the new tag, commit the changes, and push the new tag to the repository.

#### Steps to Run the Script

1. Ensure you have the necessary permissions to push changes to the repository.
2. Open a terminal and navigate to the root of the repository.
3. Run the following command:

    ```bash
    ./bin/release
    ```

4. The script will handle the rest, including fetching the latest version, updating the submodule, and pushing the new tag.

### GitHub Actions Workflow

The repository includes a GitHub Actions workflow defined in `.github/workflows/releaser.yml`. This workflow is triggered on new tags matching the pattern `v[0-9]+.[0-9]+.[0-9]+` or `v[0-9]+.[0-9]+.[0-9]+-rc[0-9]+`.

The workflow performs the following steps:

1. Checks out the repository and its recorded `stratos-ui` submodule commit.
2. Verifies that the workflow runs from a tag and that the submodule commit matches the Stratos tag with the same name.
3. Sets up Node.js, Bun, and Go using the versions specified by the checked-out Stratos submodule.
4. Builds and packages Stratos for Cloud Foundry with `make build cf` and `make release cf`.
5. Creates a new release with the packaged files.

The published `stratos-ui.zip` is a prebuilt Linux/amd64 Cloud Foundry package containing the backend binary, frontend assets, and deployment manifest. Manual workflow runs must select a tag, not a branch. A repository dispatch running on the default branch cannot produce a release.