# Deploy to Pantheon (GitHub Action)

This repository contains a GitHub action to setup code deployment from a GitHub repository to a Pantheon development or multidev environment. The Shell script that runs the actual deployment can be found in [this repo](https://github.com/padillaco/action-deploy-to-remote-repository).

## 1. Copy the Workflow Template Files

Copy the [.github](/example/.github/) folder to the WordPress root of the repository that is being setup for auto-deployments.

## 2. Actions

The [.github/actions/build-and-deploy/action.yml](.github/actions/build-and-deploy/action.yml) action file includes 4 steps:

1. Checkout Repository
2. Setup Node: Downloads and caches Node.js and adds it to the PATH
3. Install JS Dependencies and Build Theme Assets
4. Deploy to Pantheon

_Note: Steps 2 and 3 are optional, and can be removed if the WordPress theme does not require assets to be built before a deployment._

These actions are triggered when pushing to a specific branch or when a pull request is closed and merged into a branch. Feel free to adjust the branch names for the **on push** or **on pull request** trigger located at the top of the workflow file.

## 2. Replace Placeholders

Replace the following placeholders within the specified workflow/action files:

1. `[pantheon-site-slug]` → The Pantheon site slug. Example: _the-site-slug_
    - workflows/pantheon-create-multidev.yml
    - workflows/pantheon-delete-multidev.yml
    - workflows/pantheon-promote-code.yml

2. `[pantheon-site-id]` → The Pantheon site ID. Format: _XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXX_
    - actions/build-and-deploy/action.yml

3. `[theme-name]` → The WordPress theme folder that includes source files for front-end assets.
    - actions/build-and-deploy/action.yml

## 3. Configure the 'Build and Deploy to Pantheon' Action

The **Build and Deploy to Pantheon** action is located at [.github/actions/build-and-deploy/action.yml](.github/actions/build-and-deploy/action.yml). The following variables in the **Deploy to Pantheon** step can be configured:
    
- The `base_directory` variable is the path to the code within the repository that will be deployed. The default value is `.`, or the root of the repository.
- The `destination_directory` variable is the path to the destination directory on the Pantheon environment where the code will be deployed. The default value is `.`, or the WordPress install root of the Pantheon environment.
- The `exclude_list` variable is the list of files/folders that will be excluded from deployment. The default value is: `.git, .github, .gitmodules, node_modules, .ddev`

## 4. Generate an SSH Key Pair

WP Engine requires that an SSH public key be added to environment to enable GitPush. See [WP Engine: Git Version Control System](https://wpengine.com/support/git/) for more details.

1. Open the terminal and run:
    ```bash
    ssh-keygen -t rsa -m PEM -C "[repository-slug]-github" -f ~/.ssh/pantheon-deploy
    ```
    _Note 1: Replace `[repository-slug]` with the repository slug name._

    _Note 2: You can replace the `~/.ssh/pantheon-deploy` path with any path since this key pair is temporary and will be deleted after use._
    
    _Note 3: Do not set a passphrase when generating the public/private key pair._

2. Copy the private key by running:
    ```bash
    cat ~/.ssh/pantheon-deploy | pbcopy
    ```

    Go to the **Settings → Secrets and variables → Actions** page of the repository and add a new repository secret named `PANTHEON_SSH_PRIVATE_KEY` then enter the contents of the private key as the value.

3. Copy the public key by running:
    ```bash
    cat ~/.ssh/pantheon-deploy.pub | pbcopy
    ```

    From the Pantheon dashboard, in the development@padillaco.com account, go to the **Personal Settings → SSH Keys** section and add the public key.

4. Remove the local public and private key:
    ```bash
    rm ~/.ssh/pantheon-deploy ~/.ssh/pantheon-deploy.pub
    ```

---
### Each push and merged pull request into the development, staging, or production branch should now trigger a deployment.