# Linux

Filesystem: [https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
Filesystem explained: [https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)
Linux Explained for Windows Users: [https://www.dedoimedo.com/computers/ultimate-linux-guide-for-windows-users.html](https://www.dedoimedo.com/computers/ultimate-linux-guide-for-windows-users.html)

- Export to path: `export PATH=$PATH:<path-to-add>`

- Check binary dependencies: `ldd <name-of-binary>`

- Check storage: `df -h`
- Check storage of current dir: `du -sh`
- Interfaces: `ip a` or `ifconfig`
- Change dir to last dir: `cd -`

## Permissions

- Change Owner of directory and its subdirectories: `sudo chown -R user:user ./path/`

## Mouting

- To mount a network share (smb/samba) use: `sudo mount -t cifs //<address>/<path> /mnt/<path> -o user=<smbuser>,password=<password>,uid=<localuser>` (requires `cifs-utils`) (see: [https://www.thomas-krenn.com/en/wiki/Mounting_a_Windows_Share_in_Linux](https://www.thomas-krenn.com/en/wiki/Mounting_a_Windows_Share_in_Linux))
- Mount windows drive: use `ntfs-3g` (??? TODO)

## apt

Use `apt`, not `dpkg` to install local packages with `apt install ./<name>.deb`. This will also resolve package dependencies.

## Shells

- To run Bash scripts in both Zsh and fish add this shebang to the bash file: `#!/usr/bin/env bash`.
- Change the default shell: [https://wiki.archlinux.org/title/Command-line_shell#Changing_your_default_shell](https://wiki.archlinux.org/title/Command-line_shell#Changing_your_default_shell)

### Bash

### Zsh

### fish

- [https://fishshell.com](https://fishshell.com)
- Offers web based configuration with `fish_config`
- Does not support `!!` out of the box but this can be supported by installing the `bang-bang` plugin from the Oh My Fish Shell framework ([https://github.com/oh-my-fish/oh-my-fish](https://github.com/oh-my-fish/oh-my-fish))
- When using fish type `help` to open the web documentation
- Show non shortened path in prompt: `set -U fish_prompt_pwd_dir_length 0` ([https://stackoverflow.com/a/37337624](https://stackoverflow.com/a/37337624))

## Packages

- Packages for different distros: [https://pkgs.org](https://pkgs.org)

## Package Formats

- AppImage  
       - like a portable exe -> can and does not have to be installed  
       - universal software package format  
       - does not require root permissions to run  
- Flatpack  
       - [https://flathub.org](https://flathub.org)
       - Discover App on KDE seems very buggy  
       - Use `flatpak` commands  
       - If Flatpak Repo is not yet added then run `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

## systemd

Status of a service:

`systemctl status <service> --lines <lines>`
`journalctl -u <service>`

## Display Servers

See [Display Server](./DisplayServer.md).

## networking

![Linux Networking Tools](https://i.redd.it/riw36lfempv61.jpg)

## Bash

```bash
# Use this at the start of a script.
# See: https://stackoverflow.com/a/13872064

#!/bin/bash

# Use this shebang to be able to run bash scripts with fish or zsh.
#!/usr/bin/env bash

# Get output from command
variable=$(command)
variable=$(command [option…] argument1 arguments2 …)
variable=$(/path/to/command)

# For loop
for x in y ; do
    # code
done

# while loop
while ... ; do

done

# Check if string starts with characters (e.g. space).
# If a string is in "" then it will preserve leading whitespaces etc.
if [[ "$string" == " "*]]; then
    # code
fi

# Substring starts with.
if [[ "$str" == START:* ]]; then
    start=$(cut -d : -f2- <<< $str)
fi

# Replace characters in string.
# See: https://stackoverflow.com/a/2871217
# would replace each occurrence of x, y, or z with _
echo "$string" | tr xyz _
# would replace repeating occurrences of x, y, or z with a single _
echo "$string" | sed -r 's/[xyz]+/_/g'

# Replace substring in string:
# First occurence
${main_string/search_term/replace_term}

# All occurences
${main_string//search_term/replace_term}

# Grep multiple (regular) expressions (See: https://phoenixnap.com/kb/grep-multiple-strings)
cat file | grep -E 'pattern1|pattern2' > grepped.txt
```