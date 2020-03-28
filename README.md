# Ansible playbook for deploying Wordpress with Docker
## Work in progress - not ready for production use!
My take on the perfect Wordpress deployment :) specifically makes it easy to run many Wordpress sites on one server.

- Each site runs in a single Docker container
- Each site is a systemd service to make management easy
- Nginx runs on the host and provides HTTPS termination and reverse proxying to the containers
- MariaDB runs on the host and is used by all Wordpress instances
- Dehydrated provides Letsencrypt integration

## Usage
Currently only tested on Debian Buster.

### Prepare server
Only needs to be done once.
- Add server to `hosts`
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts host-from-scratch.yml --extra-vars "@sites/example.com"`

### Deploy site
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts launch-site.yml --extra-vars "@sites/example.com`

### Deactivate site
Meant for temporarily deactivating a Wordpress site without deleting it
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts deactivate-site.yml --extra-vars "@sites/example.com`

### Delete site
Irrecoverably deletes a Wordpress site and all associated data! Make sure to have a backup!
- Copy `sites/template.yml` to `sites/yourdomain.yml` and fill in the variables
- `ansible-playbook -i hosts delete-site.yml --extra-vars "@sites/example.com`

## To Do
- add remote logging
- add smtp configuration
- docker volumes
- borg agent / db dumpgs
- switch from apache image to the fpm image
- maybe: Consolidate nginx, mariadb, and docker roles into one
- maybe: chrooted sftp access so customers can edit their sites
- maybe: build own Wordpress Docker images
