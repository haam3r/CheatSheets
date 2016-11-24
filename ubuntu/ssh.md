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
    ```bash
    ssh-keyscan -t ed25519 -f hosts >> ~/.ssh/known_hosts
    ```

* To find out which entry is for a known hostname in known_hosts:
    ```bash
    ssh-keygen -H  -F <hostname or IP address>
    ```

* To delete a single entry from known_hosts:
    ```bash
    ssh-keygen -R <hostname or IP address>
    ```

#### Sources:
* [http://askubuntu.com/questions/29942/how-can-i-break-out-of-ssh-when-it-locks]
* [http://serverfault.com/questions/29262/how-to-manage-my-ssh-known-hosts-file]
