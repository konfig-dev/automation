# Konfig Automation

## General Structure

![Workflow Diagram](./images/workflow-diagram.png)

## Workflow Files

Walkthrough the functionality of each of Konfig's workflow files.

### regenerate-sdks-on-oas-change

Identifies all konfig.yaml files; then, for each, checks if the corresponding OpenAPI spec was modified. If so, calls the `regenerate-sdks` workflow for that konfig.yaml directory.

### regenerate-sdks

First, fixes the OpenAPI spec, then lints it (unless linting is disabled by config). Next, regenerates SDKs and creates a changeset file. Then, pushes these changes to the PR. If there are any SDK submodules, first those will be pushed to a PR created in the corresponding submodule repository.

Next, the regenerated SDKs are tested in a docker container. If they pass, then the PRs are merged. First, any submodule PRs are merged, and the submodule references are updated and pushed to the main PR, if applicable; then, the main repo's PR is merged.

### release

Identifies all konfig.yaml files; then, for each, calls the `bump` workflow and `publish` workflow for that konfig.yaml directory.

### bump

First checks for a changeset file, and only runs subsequent steps if one is found. If applicable, submodules are synced to make sure everything is up-to-date. Next, the changeset file is deleted and the SDK versions are bumped.

Then, pushes the changes to a bump PR in the main repo. Once again, if there are any SDK submodules, first those will be pushed to a PR created in the corresponding submodule repository with the same branch name.

Finally, unless automatic merging of bump PR is disabled in config, main repo's bump PRs is merged. (Submodule bump PRs have not yet been merged.)

### publish

First checks for NO changeset files, and only runs subsequent steps if none are found. Next, if applicable, merges submodule bump PRs, then syncs submodules to update the main repo with the newly merged commits. This is all done in the `pre-publish` job.

Then, the SDKs are tested before merging. If the tests pass, then all the necessary setup for publishing to the package managers is performed, followed by the publishing of the SDKs.
