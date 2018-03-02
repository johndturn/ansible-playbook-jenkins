# Ansible Playbook - Deploy / Configure Jenkins

This playbook is meant to be a starting point for anyone looking to deploy, serve, and configure their Jenkins instance. It includes the following roles:

* `jenkins_deploy` - Installs Jenkins and its deps on the server (assuming Ubuntu 16.04)
* `jenkins_nginx` - Installs and configures Nginx and LetsEncrypt on said server in order to stand as a reverse proxy between the normal HTTPS port and Jenkins default port (8080). Serves Jenkins over the web with HTTPS via LetsEncrypt
* `jenkins_configure` - Set up Jenkins with your desired plugins and jobs

## Using this Playbook

This playbook is meant to be a **starting point**, not an out-of-the-box solution. Fork this repo, or download the source code and modify it till your heart's content.

Currently, it demonstrates how to set up Jenkins with plugins and configure 1 Jenkins job, reading from a BitBucket Repo. This is only meant to serve as an example. If you're not using BitBucket, then by all means rip it all out (Jenkins connectino to GitHub is by far superior anyway).

In prepping this playbook for your own use, there are several files / variables that you should be extra aware of, in order to have a successful deploy:

* `inventories/` - This directory should contain your different environment files, pointing to the public IP addresses for your instances running Jenkins. These should be edited, and other environments can be added.
* `deploy_jenkins.yml` - The main entry point for the playbook. Be aware of the variables that exist in this file, and add/remove/edit them accordingly.
* `roles/jenkins_nginx/files/nginx.conf` - Main Nginx configuration file. Already has some default configurations in it, but can be added to or edited.
* `roles/jenkins_nginx/templates/nginx-http.conf.j2` - Nginx server block for all traffic coming in on port 80. Meant to only allow traffic to LetsEncrypt's challenge. Otherwise, reroute to HTTPS (This file can be left unchanged)
* `roles/jenkins_nginx/templates/jenkins-nginx.conf.j2` - Server block for all Jenkins traffic coming in over HTTPS. Feel free to edit this, though it's already parameterized for your correct domain.

## Future Improvements

This playbook has a lot of places where improvements can (and should) be made. It you'd like to contribute, please fork this repo, and open a PR. I'll gladly review it and merge it.

Specifically, some things that should be changed include:

* Look at the places where we force a restart of Jenkins, and consider changing those hard restarts to Handlers
* Fix the polling of Jenkins to tell when it has successfully restarted after installing new plugins
* Include the creation of different Jenkins Credentials in Ansible to avoid having to manually set up Jenkins Creds

## License

MIT / BSD
