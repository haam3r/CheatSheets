# SSH settings and useful commands

* In `/etc/ssh/ssh_config` set:
    ```bash
    Host *
        # Send keepalives to prevent ssh sessions from dying
        ServerAliveInterval 15
        # Ubuntu idiosyncracy, is to by default hash hostnames in known_hosts,
        # but we want to autocomplete those names
        HashKnownHosts no
    ```

* Add ssh host keys from machines specified in a file. Only add ed25519 keys:
    `ssh-keyscan -t ed25519 -f hosts >> ~/.ssh/known_hosts`
