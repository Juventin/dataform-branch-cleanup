# Dataform : Cleanup after branch deletion

## Description

This GitHub Action cleans up resources in Google Cloud Platform (GCP) after a branch is **deleted**. 

Managing multiple development branches in a Dataform project can quickly become cumbersome, especially when it comes to cleaning up resources after a branch is deleted. This GitHub Action automates the cleanup process, ensuring that your Google Cloud Platform (GCP) resources are efficiently managed.

**BigQuery Schemas:**
- Automatically deletes all BigQuery schemas with the corresponding branch name suffix.

**Dataform Workspaces:**
- Automatically deletes the Dataform workspace associated with the deleted branch.

> Imagine your team is working on multiple features simultaneously, each in its own branch. You can configure Dataform to make each branch create its own BigQuery schemas. Once a feature is completed and the branch is merged and deleted, this action will automatically clean up the associated resources. This ensures that your GCP environment remains clean and free of unused resources, saving costs and reducing clutter.

## Prerequisites

Before using this action, ensure you have a Google Cloud Platform (GCP) project using **BigQuery** and **Dataform linked to a GitHub repository**.

Here is the Dataform configuration necessary:
- **General configuration**
  - *Make sure your Dataform project is linked to the GitHub repository using this action.*

- **Workspace compilation overrides**
  - Schema suffix: `${workspaceName}`

## Usage

Once the setup is complete, this action will automatically run whenever a branch is **deleted** in your repository. It will clean up the corresponding BigQuery schemas and Dataform workspaces in GCP.

### Setup

1. **Create a service account in GCP**:
   - Go to the [GCP Console](https://console.cloud.google.com/).
   - Navigate to IAM & Admin > Service Accounts.
   - Create a new service account with the following permissions:
     - **BigQuery Data Owner**
     - **Dataform Editor**
   - Generate a JSON key for the service account and save it.

2. **Add the service account key to GitHub secrets**:
   - Go to your GitHub repository.
   - Navigate to *Settings > Secrets and variables > Actions > Secrets*.
   - Add a new **Repository secret** with the name `SA_KEY` and paste the JSON key content.

3. **Create a workflow file**:
   - In your repository, create a new file at `.github/workflows/cleanup.yml` with the following content:

```yaml
name: Cleanup After Branch Deletion

on:
  delete:
    branches:
      - "**"

jobs:
  cleanup:
    runs-on: ubuntu-latest
    steps:
    - uses: 'juventin/dataform-branch-cleanup@v1.0.0'
      with:
        project_id: 'your-gcp-project-id'
        dataform_repo_name: 'your-dataform-repo-name'
        dataform_repo_location: 'your-dataform-repo-location' (e.g. 'us-central1')
        SA_KEY: ${{ secrets.SA_KEY }}
```

# Author
[@juventin](https://github.com/Juventin)
