# Debian

- Download netinst ISO from: [https://www.debian.org/download](https://www.debian.org/download)

```bash
# Add user to sudoers (only if a password was set for the root user during installation).
# See: https://wiki.debian.org/sudo/
su -
usermod -aG sudo <username>
su <username>

# Logout and Login again.
```

```bash
# Add mirrors.
nano /etc/apt/sources.list

# Add the following:
deb http://ftp.us.debian.org/debian stretch main contrib non-free
deb-src http://ftp.us.debian.org/debian stretch main contrib non-free
```