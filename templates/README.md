# 📘 Automation for CAMARA API Repository Creation

The automation workflow "Setup New Repository" simplifies the process of setting up a new CAMARA API repository, ensuring consistency across teams and reducing manual steps.

This document describes how to use the GitHub Action in the `Template_API_Repository` to automate the setup of new API repositories within the [CAMARA GitHub organization](https://github.com/camaraproject).

---

## 🚀 Purpose

To automate the initial administrative setup of a new API repository, ensuring consistency and reducing manual work. This includes:

- Creating a new public repository based on the template
- Setting repo description, topics, and metadata
- Creating and configuring teams
- Assigning repository permissions
- Updating the README with project-specific details
- Adding CODEOWNERS
- Creating initial administrative issues
- Cleaning up template files from the new repository

---

## ⚙️ How to Use

### 🛠 Prerequisites for Administrators

Before running the workflow, ensure the following are set up:

1. **Environment Setup**:

   - A GitHub environment named `repo-setup` must exist in the template repository.

2. **Fine-Grained Personal Access Token (FGPAT)**:

   - A secret named `GH_REPO_CREATE_TOKEN` must be stored in the `repo-setup` environment.
   - This token must be a **fine-grained personal access token** with access to both the template and target repositories.

   #### 🔐 Required Token Permissions:

   - **Contents**: Read and write
   - **Issues**: Read and write
   - **Metadata**: Read-only
   - **Administration**: Read and write

   #### 🔧 How to Create a Fine-Grained Token:

   1. Go to [GitHub Developer Settings – Fine-grained personal access tokens](https://github.com/settings/personal-access-tokens)
   2. Click **"Generate new token (fine-grained)"**
   3. Set **Resource owner** to your GitHub username (for testing) or organization (camaraproject)
   4. Choose **repositories to access**: all repositories (otherwise newly created repositories are not covered)
   5. Under **Repository permissions**, set:
      - Contents → Read and write
      - Issues → Read and write
      - Metadata → Read-only
      - Administration → Read and write
      - Generate the token and copy it
   6. Add it as the `GH_REPO_CREATE_TOKEN` secret under **Settings > Environments > repo-setup**

---

### ▶️ Running the Workflow

1. Go to the `Actions` tab of [`Template_API_Repository`](https://github.com/camaraproject/Template_API_Repository/actions)
2. Select the **"Setup New Repository"** workflow
3. Use the `workflow_dispatch` form to input the following values:

#### 🔤 Inputs

| Input                  | Required | Description                                                                                                  |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------------ |
| `repo_name`            | ✅        | Name of the new repository to create                                                                         |
| `subproject_name`      | ❌        | Optional subproject or working group name                                                                    |
| `repo_wiki_page`       | ✅        | URL of the new repository's wiki page                                                                        |
| `subproject_wiki_page` | ❌        | URL of the subproject’s wiki page (if applicable)                                                            |
| `mailinglist_name`     | ✅        | Mailing list associated with the repo                                                                        |
| `initial_codeowners`   | ✅        | Space-separated GitHub usernames (with `@`). Ensure each user exists and can be invited to the organization. |

#### 💡 Example Input:

```text
repo_name: my-new-api
subproject_name: mobility
repo_wiki_page: https://github.com/camaraproject/my-new-api/wiki
subproject_wiki_page: https://github.com/camaraproject/mobility/wiki
mailinglist_name: mobility@lists.camaraproject.org
initial_codeowners: @alice @bob @charlie
```

---

## 🧱 What the Workflow Does

This GitHub Actions workflow is defined in the `Template_API_Repository`, which serves as the starting point for all new API repositories in the CAMARA project.

- Creates a new public repository using this template
- Waits until the repo is accessible
- Sets metadata:
  - Description, homepage URL, topic `sandbox-api-repository`
  - Enables discussions and issues; disables wiki
- Creates two teams:
  - `repo_name_maintainers` (under `maintainers`)
  - `repo_name_codeowners` (under `codeowners`)
- Assigns permissions:
  - `triage` to maintainers
  - `push` to codeowners
  - `maintain` to admins
- Updates `README.md` using placeholder replacement
- Generates and commits a `CODEOWNERS` file from template
- Syncs all rulesets from the template repository
- Opens two issues:
  - Initial administrative tasks
  - Initial tasks for codeowners
- Deletes listed template files from the new repository

---

## ✅ What to Do After the Workflow

- Follow the instructions and checklist in the created issue "New Repository - Initial administrative tasks #1", especially:
  - Review the newly created repository and ensure all README placeholders and metadata have been populated correctly
  - Confirm that the CODEOWNERS file includes valid GitHub usernames. If users are not part of the organization, team invitations may not succeed
  - Delete `.github/workflows/setup-new-repo.yml` manually from the new repository, as it is not  removed automatically
  - Note: Improvements of this checklist should go into `/templates/issues/initial-admin.md`

---

## 📌 Future Enhancements

- Set the correct labels for issues and pull requests within the new repository
- Adding linting workflow (based on reusable workflow from `Tooling` repository)
- Validate usernames/org membership during CODEOWNERS setup
- Dry-run support for validation without creation
- Move the workflow out of the template repository into e.g. `.github` or `Tooling` repository (requires some rewriting of the workflow)

---

For questions or feedback, open an issue in [`Template_API_Repository`](https://github.com/camaraproject/Template_API_Repository/issues) or reach out via the mailing list [adm@lists.camaraproject.org](mailto\:adm@lists.camaraproject.org).
