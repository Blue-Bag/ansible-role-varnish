# Ansible Role: Varnish

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-varnish.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-varnish)

An Ansible Role that installs Varnish on RedHat/CentOS or Debian/Ubuntu Linux.

## Requirements

Requires the EPEL repository on RedHat/CentOS (you can install it using the `geerlingguy.repo-epel` role).

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    varnish_config_path: /etc/varnish

The path in which Varnish configuration files will be stored.

    varnish_use_default_vcl: true

Whether to use the included (simplistic) default Varnish VCL, using the backend host/port defined with the next two variables. Set this to `false` and copy your own `default.vcl` file into the `varnish_config_path` if you'd like to use a more complicated setup. If this variable is set to `true`, all other configuration will be taken from Varnish's own [default VCL](https://www.varnish-cache.org/trac/browser/bin/varnishd/default.vcl?rev=3.0).

    varnish_default_vcl_template_path: default.vcl.j2

The default VCL file to be copied (if `varnish_use_default_vcl` is `true`). Defaults the the simple template inside `templates/default.vcl.j2`. This path should be relative to the directory from which you run your playbook.

    varnish_default_backend_host: "127.0.0.1"
    varnish_default_backend_port: "8080"

Some settings for the default "default.vcl" template that will be copied to the `varnish_config_path` folder. The default backend host/port could be Apache or Nginx (or some other HTTP server) running on the same host or some other host (in which case, you might use port 80 instead).

    varnish_listen_port: "80"

The port on which Varnish will listen (typically port 80).

    varnish_secret: "14bac2e6-1e34-4770-8078-974373b76c90"

The secret/key to be used for connecting to Varnish's admin backend (for purge requests, etc.).
Generate one at https://www.uuidgenerator.net/
or use {{ hostname | to_uuid }}
$uuidgen


    varnish_admin_listen_host: "127.0.0.1"
    varnish_admin_listen_port: "6082"

The host and port through which Varnish will accept admin requests (like purge and status requests).

    varnish_storage: "file,/var/lib/varnish/varnish_storage.bin,256M"

How Varnish stores cache entries (this is passed in as the argument for `-s`). If you want to use in-memory storage, change to something like `malloc,256M`. Please read Varnish's [Getting Started guide](https://www.varnish-software.com/static/book/Getting_started.html) for more information.

## Dependencies

None.

## Example Playbook

    - hosts: webservers
      vars_files:
        - vars/main.yml
      roles:
        - { role: geerlingguy.varnish }

*Inside `vars/main.yml`*:

    varnish_secret: "[secret generated by uuidgen]"
    varnish_default_backend_host: 81
    ... etc ...

## License

MIT / BSD

## Notes:

Updated as per instruction at https://packagecloud.io/varnishcache/varnish41/install#manual

see https://twitter.com/MoritzBunkus/status/834149922010255360

## Author Information

This role was created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
Incorporates monitoring tip from Steven Jones - see https://www.computerminds.co.uk/articles/monitoring-varnish
