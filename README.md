# GitHub Actions Reuseable Workflows

The main purpose of this repository is to avoid duplication when creating a workflow by reusing existing workflows as per
[Reusing workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows). See below on how to call each
workflow and what inputs and/or secrets are needed.

See example below on how to use this repository workflows:

```yaml
name: "Call a reusable workflow"

on:
  pull_request:
    branches:
      - main

jobs:
  call-workflow:
    uses: kwame-mintah/github-actions-reusable-workflows/.github/workflows/<name>.yml@v1 # could be branch name or git commit id
```

# Workflows

## [🚧 Bump version](.github/workflows/repository-run-version-bump.yml)

This job will utilize commitizen to bump and create a new tag within the repository. This requires [`.cz.toml`](https://commitizen-tools.github.io/commitizen/config/)
to be present. See below for an example:

```toml
[tool]
[tool.commitizen]
bump_message = "release $current_version → $new_version [skip-ci]"
name = "cz_conventional_commits"
tag_format = "v$version"
version = "0.0.0"
update_changelog_on_bump = true
annotated_tag = true
```

## [🚀 Push Docker image to GCP Artifact Registry](.github/workflows/gcp-docker-build-and-push.yml)

This job will check out the repository and push a docker image to the chosen GCP Artifact Registry using
[setup-gcloud](https://github.com/google-github-actions/setup-gcloud/tree/v2.1.2) action.

The following repository inputs need to be set:

| Secret                       | Description                                 |
| ---------------------------- | ------------------------------------------- |
| GCP_PROJECT_ID               | The default project to manage resources in. |
| GCP_REGION                   | The default region to manage resources in.  |
| GCP_REGISTRY_REPOSITORY_NAME | The repository name within the registry     |
| DOCKER_IMAGE_NAME            | The name of the docker image built          |

The following repository secrets need to be set:

| Secret                         | Description                                         |
| ------------------------------ | --------------------------------------------------- |
| GCP_GITHUB_SERVICE_ACCOUNT_KEY | The json private key for the GitHub service account |

## [🛸 GCP Cloud Run Deploy](.github/workflows/gcp-cloud-run-deploy.yml)

This job will check out the repository and deploy the cloud run function utilizing
the same GitHub action mentioned above.

The following repository secrets need to be set:

| Secret                         | Description                                         |
| ------------------------------ | --------------------------------------------------- |
| GCP_GITHUB_SERVICE_ACCOUNT_KEY | The json private key for the GitHub service account |

The following repository variable needs to be set:

| Secret                       | Description                                 |
| ---------------------------- | ------------------------------------------- |
| GCP_CLOUD_RUN_NAME           | The name of the cloud run service to deploy |
| GCP_PROJECT_ID               | The default project to manage resources in. |
| GCP_REGION                   | The default region to manage resources in.  |
| GCP_REGISTRY_REPOSITORY_NAME | The repository name within the registry     |
| DOCKER_IMAGE_NAME            | The name of the docker image built          |
