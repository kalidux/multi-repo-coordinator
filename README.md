# Multi-Repo Integration Workflow

## Overview

This repository includes a GitHub Actions workflow designed to automate the integration of multiple repositories. The workflow dynamically handles the checkout of repositories and branches as specified in an `integration.yaml` file, ensuring that the correct repositories and branches are targeted for building and testing.

## Workflow Features

### Dynamic Matrix for Repository and Branches

A key feature of this workflow is the dynamic matrix created from the `integration.yaml` file. The workflow automatically counts the repositories and branches defined in the file and sets up a matrix job to handle each one individually. This eliminates the need for hardcoded values and allows for scalable integration of any repositories listed in the `integration.yaml` file.

1. **Job to Count Repositories**: The first job (`count_pending_prs`) counts the repositories defined in the `integration.yaml` file and extracts their names and branches. These are used as inputs for the following jobs.
   
2. **Dynamic Matrix Setup**: The `build_repositories` job leverages a matrix strategy where each combination of repository name and branch is used as an input to the subsequent steps. This ensures that the right repository and branch are checked out for each build process.

### Checkout Repositories and Branches

After setting up the matrix, the workflow checks out each repository and the corresponding branch from GitHub. It automatically clones the repository and switches to the correct branch if it is specified in the `integration.yaml` file.

1. **Repository Validation**: The workflow verifies that the repository and branch exist in the `integration.yaml` file before proceeding. If the repository or branch is not found, the job is skipped.
   
2. **Checkout Process**: If the repository and branch are valid, the workflow clones the repository and checks out the corresponding branch. This is done automatically for each repository and branch combination defined in the matrix.

### Generic Build and Test

Once the correct repository and branch are checked out, the build and test steps are executed using two generic scripts: `build.sh` and `test.sh`.

1. **Build Process**: The build process is handled by the `build.sh` script. This script is expected to contain the build commands specific to each project or repository. The `build.sh` script is executed automatically, and developers are responsible for defining the necessary build steps in it.

2. **Testing Process**: Similarly, the `test.sh` script is used to run tests. This script should include the testing commands that are relevant to each project or repository. It is automatically executed after the build process.

The use of `build.sh` and `test.sh` allows for a generic build and test process across multiple repositories, making it easier to integrate new repositories without modifying the workflow each time.

### Limitations and Future Improvements

- **Merge Management**: Currently, the workflow does not handle the merge process, which needs to be implemented separately.
  
- **Further Improvements**: There are several improvements and optimizations that can be added, such as handling merge conflicts, notifying when builds fail, and integrating more advanced testing pipelines.

### Goal of the Workflow

The primary goal of this workflow is to create a **generic** and **scalable** solution for handling multi-repository builds. The flexibility provided by the dynamic matrix and generic build/test scripts enables easy integration of new repositories and branches with minimal configuration.

---
