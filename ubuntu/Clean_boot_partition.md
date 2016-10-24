# Cleaning Ubuntu boot partition

## Command line method

1. First check your kernel version, so you won't delete the in-use kernel image, running:
    ```bash
    uname -r
    ```

1. Now run this command for a list of installed kernels:
    ```bash
    sudo dpkg --list 'linux-image*'
    ```

1. And delete the kernels you don't want/need anymore by running this:
    ```bash
    sudo apt-get remove linux-image-VERSION
    ```
    NB! Replace VERSION with the version of the kernel you want to remove.

1. When you're done removing the older kernels, you can run this to remove every package you won't need anymore:
    ```bash
    sudo apt-get autoremove
    ```

1. And finally you can run this to update grub kernel list:
    ```bash
    sudo update-grub
    ```

**Note:**

```bash
sudo dpkg --list 'linux-image*' | grep ^ii
```

makes it a little easier to see just the installed kernels. Also I think the update-grub is harmless but not strictly necessary, that is run automatically when you uninstall a kernel.

## Second method

**NOTE: this is only if you can't use apt to clean up due to a 100% full /boot**
If apt-get isn't functioning because your /boot is at 100%, you'll need to clean out /boot first. This likely has caught a kernel upgrade in a partial install which means apt has pretty much froze up entirely and will keep telling you to run apt-get -f install even though that command keeps failing.
Get the list of kernel images and determine what you can do without. This command will show installed kernels except the currently running one:

```bash
sudo dpkg --list 'linux-image*'|awk '{ if ($1=="ii") print $2}'|grep -v `uname -r`
```

Note the two newest versions in the list. You don't need to worry about the running one as it isn't listed here. You can check that with `uname -r`.
Craft a command to delete all files in /boot for kernels that don't matter to you using brace expansion to keep you sane. Remember to exclude the current and two newest kernel images. Example:

```bash
sudo rm -rf /boot/*-3.2.0-{23,45,49,51,52,53,54,55}-*
```

```bash
sudo apt-get -f install #to clean up what's making apt grumpy about a partial install
```


**NB!** If you run into an error that includes a line like `Internal Error: Could not find image (/boot/vmlinuz-3.2.0-56-generic)`, then run the command (with your appropriate version):

```bash
sudo apt-get purge linux-image-3.2.0-56-generic
```

Finally,

```bash
sudo apt-get autoremove
```

 to clear out the old kernel image packages that have been orphaned by the manual boot clean.

## Suggestions

* Suggestion: run `sudo apt-get update` and `sudo apt-get upgrade` to take care of any upgrades that may have backed up while waiting for you to discover the full /boot partition.
* Suggestion 2: Review <https://help.ubuntu.com/community/AutomaticSecurityUpdates> and consider setting `Unattended-Upgrade::Remove-Unused-Dependencies` to true in `/etc/apt/apt.conf.d/50unattended-upgrades`. This will be the equivalent of running autoremove after each security updates to be sure you clean out unused kernels but will also remove other things it thinks are unused saving you from this problem in the future.

## FAQ

### May I ask why to exclude the last two newest kernel images

* This way I have the latest for the next reboot and then the one before just in case something breaks in that one. Usually I have plenty of room so it doesn't hurt to have a few and it satisfies my paranoia for not having enough backup options in any given scenario.

* This one-liner from <http://ubuntuforums.org/showthread.php?t=1435818> did the trick for me:
  ```bash
  dpkg --get-selections|grep 'linux-image*'|awk '{print $1}'|egrep -v "linux-image-$(uname -r)|linux-image-generic" |while read n;do apt-get -y remove $n;done
  ```

**NB!** However, this only worked once! The second time I ran it, my computer no longer had a kernel (see comment below). Be careful with this command as I spent about 10 hours figuring out what happened and then replacing the kernel.

* This will remove the current linux-image-extra and the version-less linux-image-server. I would suggest using:

```bash
dpkg --get-selections | grep 'linux-image.*-[0-9].*' | awk '{print $1}' | egrep -v "$(uname -r)"
```

look at the list carefully and only then append:

```bash
| while read n; do apt-get -y remove $n; done
```


## Source

<http://askubuntu.com/questions/345588/what-is-the-safest-way-to-clean-up-boot-partition>
