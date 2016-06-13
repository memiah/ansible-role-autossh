AutoSSH
=======

An Ansible Role that installs and configures an single autossh connection, 
allowing port forwarding from the local server to a remote server.

Requirements
------------

We assume that the SSH user exists and has the correct permissions to 
access on the remote server.

Role Variables
--------------

Available variables are listed below, along with default values (see 
`defaults/main.yml`):

    autossh_path: "/usr/bin/autossh"

Path to autossh.

    autossh_systemd_dir: "/etc/systemd/system/"

Path to system dir.

    autossh_systemd_filename: "autossh.service"
    
Filename used in the systemd_dir to store autossh service.
    
    autossh_ssh_dir: "/root/.ssh"
    
Directory to store SSH configuration.

    autossh_connections:
      - guid: "example" # Simple lowercase unque identifier (a-z_-).
        user: "" # Username used to connect to remote server.
        server: "" # IP / hostname of remote server.
        local_port: "" # Local port to forward.
        dest_server: "127.0.0.1" # IP / hostname to use on the remote server, this will most likely be localhost 127.0.0.1.
        dest_port: "" # Port on the remote server to connect to.
   
Add a set of SSH connection properties per connection.
    
    autossh_ssh_server_key_type: "ecdsa"
    
Key type of the remote server.
    
    autossh_known_hosts_file: "{{ autossh_ssh_dir }}/known_hosts"

Path to known hosts file.
    
    autossh_ssh_public_key: |
      ssh-rsa AAAA...

SSH public key used to connect to the remote server. (Optional)
    
    autossh_ssh_private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----
 
SSH private key used to connect to the remote server.
    
    autossh_ssh_keys:
      public:
        filename: "autossh_id_rsa.pub"
        content: "{{ autossh_ssh_public_key }}"
      private:
        filename: "autossh_id_rsa"
        content: "{{ autossh_ssh_private_key }}"

SSH key-pair file names and contents.

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
      - guid: "example"
        user: "username"
        server: "remote.server"
        local_port: "33061"
        dest_port: "3306"
        dest_server: "127.0.0.1"
    autossh_ssh_private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----

Forward local port `33061` to port `3306` on `remote.server`, connecting
via SSH as `username@remote.server`.

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2016 by [Memiah Limited](https://github.com/memiah).
