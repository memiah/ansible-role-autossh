AutoSSH
=======

An Ansible Role that installs and configures multiple autossh connections, 
allowing port forwarding from the local server to remote servers via SSH.

Requirements
------------

We assume that the SSH user exists and has the correct permissions to 
access on the remote server. A corresponding SSH key must also be present 
and available on the local server (`/root/.ssh/id_rsa` or defined per 
connection configuration).

Role Variables
--------------

Available variables are listed below, along with default values (see 
`defaults/main.yml`):

    autossh_connections:
      - id: "example" # Simple unique connection identifier (characters: "a-z,0-9,-").
        user: "" # Username used to connect to remote server.
        server: "" # IP / hostname of remote server.
        server_key_type: "" # Key type of the remote server, defaults to autossh_default_server_key_type. (Optional)
        local_port: "" # Local port to forward.
        dest_server: "" # IP / hostname to use on the remote server, defaults to autossh_default_dest_server. (Optional)
        dest_port: "" # Port on the remote server to connect to.
        identity_file: "" # Path to SSH private key, defaults to autossh_default_ssh_key_path. (Optional)

Add a set of SSH connection properties per connection. Multiple connections
can be listed here.

    autossh_path: "/usr/bin/autossh"

Path to autossh.

    autossh_ssh_dir: "/root/.ssh"

Directory to store SSH configuration.

    autossh_systemd_dir: "/etc/systemd/system/"

Path to systemd system dir.

    autossh_known_hosts_file: "{{ autossh_ssh_dir }}/known_hosts"

Path to known hosts file.

    autossh_default_server_key_type: "ecdsa"

Default key type of the remote server. Override this value for an single 
connection using the `server_key_type` option in the `autossh_connections` 
list. 

    autossh_default_dest_server: "127.0.0.1"

Default IP / hostname to use on the remote server, this will most likely 
be localhost / 127.0.0.1. Override this value for an single connection 
using the `dest_server` option in the `autossh_connections` list. 

    autossh_default_identity_file: "/root/.ssh/id_rsa"

Path to file from which the identity (private key) for public key 
authentication is read. The default is `/root/.ssh/id_rsa` for 
protocol version 2. Override this value for an single connection using 
the `identity_file` option in the `autossh_connections` list. 

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
         - { role: memiah.autossh }

*Inside `vars/main.yml`*:

    autossh_connections:
      - id: "example"
        user: "username"
        server: "remote.server"
        local_port: "33061"
        dest_port: "3306"

Forward local port `33061` to port `3306` on `remote.server`, connecting
via SSH as `username@remote.server`.

This will create a new service named `autossh-example.service` 
*(autossh-{id}.service)* which can be managed by systemctl:

    systemctl status autossh-example.service
    systemctl restart autossh-example.service
    ...

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2016 by [Memiah Limited](https://github.com/memiah).
