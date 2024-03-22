# action-wbox

This GitHub Action provides a composite action to build and release tools for Windows.
It automates the process of checking out the source code, installing dependencies, building the tool, uploading the build artifacts, and creating a GitHub release.

## Inputs

| Input              | Description                                                                       | Required |
|--------------------|-----------------------------------------------------------------------------------|:--------:|
| `tool-name`        | The name of the tool to be built.                                                 |   Yes    |
| `tool-version`     | The version of the tool to be built.                                              |   Yes    |
| `artifact-name`    | The name of the artifact (e.g., `"tool-name-x.x-windows-x86-64.zip"`).            |   Yes    |
| `release-token`    | Token for GitHub release.                                                         |   Yes    |
| `tool-dependencies`| Dependencies required for the tool, separated by spaces (e.g., `"dos2unix grep"`).|   No     |

## Runs

This action uses the `composite` runs type.

## Steps

1. **Check out source code**: Checks out the source code for the build process.
2. **Install Dependencies**: Installs the necessary dependencies specified by `tool-dependencies`.
3. **Build**: Executes the build script for the specified `tool-name`.
4. **Upload Artifacts**: Uploads the build artifacts to GitHub.
5. **Upload Build to Release**: Creates a GitHub release and uploads the build artifacts to the release.

## Example Usage

### GNU make for Windows

Trigger on tags like `make-*`.

```yaml
on:
  push:
    tags:
      - 'make-*'

jobs:
  build-and-release:
    name: GNU make
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Build and Release GNU make
        uses: tisvis/action-wbox@v1
        with:
          tool-name: 'make'
          tool-version: '4.4'
          artifact-name: 'make-4.4-windows-x86-64.zip'
          tool-dependencies: 'dos2unix'
          release-token: ${{ secrets.WBOX_RELEASE_TOKEN }}
