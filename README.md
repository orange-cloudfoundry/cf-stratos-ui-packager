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

1. Checks out the repository.
2. Sets up Go.
3. Packages the `stratos-ui` submodule.
4. Creates a new release with the packaged files.