# Deploy to Pantheon

Uses [action-deploy-to-remote-repository](https://github.com/padillaco/action-deploy-to-remote-repository) and Pantheon [terminus](https://docs.pantheon.io/terminus) to deploy files/folders from a local GitHub action repository to a Pantheon environment.

_Notes_:

- This action depends upon the upstream action [action-deploy-to-remote-repository](https://github.com/padillaco/action-deploy-to-remote-repository) and it's associated requirements (eg. [ssh_key](https://github.com/padillaco/action-deploy-to-remote-repository#ssh_key).
- The `develop` branch is deployed to the Pantheon `dev` environment (via a git commit to Pantheon's `master` branch), and all other branch names are deployed to a Pantheon multidev environment of the same name (unless overridden with [pantheon_env_name](#pantheon_env_name)).

## Usage

Example deploy to a remote repository:

```yml
name: Deploy to Pantheon Live

on:
  push:
    branches:
      - develop

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    # Build your project here
    # ...

    - name: Deploy to Pantheon
      uses: padillaco/action-deploy-to-pantheon@main
      with:
        pantheon_site: 'your-site-name'
        pantheon_site_id: '12345678-YOUR-SITE-ID00-123456789123'
        pantheon_machine_token: ${{ secrets.SSH_KEY }}
        terminus_version: '3.3.0'
        ssh_key: ${{ secrets.SSH_KEY }}
        base_directory: '/'
        destination_directory: '/'
        exclude_list: '.git, .github, .gitmodules, node_modules'

```

## Inputs

> Specify using `with` keyword. See all upstream inputs for [action-deploy-to-remote-repository](https://github.com/padillaco/action-deploy-to-remote-repository).

### `base_directory`

- Specify the base directory to sync from.
- Accepts a string.
- Defaults to the root of the repository (`.`). **NOTE** You likely want a
  trailing slash if you're syncing a subdirectory. (eg. `wp-content/`)
- Inherited from `action-deploy-to-remote-repository`.

### `destination_directory`

- Specify the destination directory to sync to.
- Accepts a string.
- Defaults to the root of the remote repository (`.`).
- Inherited from `action-deploy-to-remote-repository`.

### `exclude_list`

- Specify a comma-separated list of files and directories to exclude from sync.
- Accepts a string. (e.g. `.git, .gitmodules`)
- Defaults to `.git, .gitmodules, .pantheon`.
- Inherited from `action-deploy-to-remote-repository`.

### `ssh_key`

- SSH key to use for remote repository authentication.
- Accepts a string (private key).
- Required.
- Inherited from `action-deploy-to-remote-repository`.

### `pantheon_site`

- Specify the name of the Pantheon site to deploy to.
- Accepts a string.
- Required.

### `pantheon_site_id`

- Specify the ID of the Pantheon site to deploy to.
- Accepts a string.
- Required.

### `pantheon_machine_token`

- Specify the Pantheon machine token to use.
- Accepts a string.
- Required.

### `terminus_version`

- Specify the version of [terminus](https://docs.pantheon.io/terminus) to use.
- Accepts a string.
- Defaults to `'3.3.0'`.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed
recently.

## Credits

This project was created by [Alley Interactive](https://github.com/alleyinteractive).

- [Ben Bolton](https://github.com/benpbolton)
- [All Contributors](https://github.com/alleyinteractive/action-deploy-to-pantheon/graphs/contributors)

## License

The GNU General Public License (GPL) license. Please see [License File](LICENSE)
for more information.
