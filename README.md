# Ansible Playbook for deploying Wordpress with Docker
My take on the perfect Wordpress deployment :) specifically makes it easy to run many Wordpress sites on one server.

- Each site runs in a single Docker container
- Each site is a systemd service to make management easy
- Nginx runs on the host and provides HTTPS termination and reverse proxying to the containers
- MariaDB runs on the host and is used by all Wordpress instances
- Dehydrated provides Letsencrypt integration
- Volumes are mounted to the host for easy access to site data
- Each site gets a system user with SFTP access to their webroot

## Project status
As of now (2020-05-01), this project is in a working state, but has not been tested on a larger scale. There are still improvements to be made (see "To do"), but it can be used just fine in its current state.

## Usage
Currently only tested on Debian Buster.

### Prepare server
Only needs to be executed once on a bare server. Sets up everything necessary to deploy Wordpress sites.
- Add server to the `hosts` file, preferably to its own section
- Add hosts section name to the `hosts: ` variable in `host-from-scratch.yml`
- `ansible-playbook -i hosts host-from-scratch.yml`

### Deploy site
Creates a new site or re-activates it after deactivation
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts launch-site.yml --extra-vars "@sites/example.com"`

### Deactivate site
Meant for temporarily deactivating a Wordpress site without deleting it. Run `launch-site.yml` to recover it while retaining all of its data
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts deactivate-site.yml --extra-vars "@sites/example.com"`

### Delete site
Irrecoverably deletes a Wordpress site and all associated data! Make sure to have a backup!
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts delete-site.yml --extra-vars "@sites/example.com"`

## To Do
- add remote logging
- add smtp configuration
- ~~add delete-site playbook~~
- ~~docker volumes~~
- borg agent / db dumpgs
- switch from apache image to the fpm image
- ~~maybe: Consolidate nginx, mariadb, and docker roles into one~~
- ~~maybe: chrooted sftp access so customers can edit their sites~~
- maybe: build own Wordpress Docker images
