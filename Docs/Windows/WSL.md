# WSL

Windows Subsystem for Linux

## Reset password of user (Ubuntu)

- Press `WIN` + `X`, `A`
- Type into Powershell: `ubuntu config --default-user root` (Next WSL command line will log in as root without asking for password)
- In WSL shell type: `passwd <username>` to reset the password for `username`
- Type into Powershell: `ubuntu config --default-user <username>` (Next WSL command line will log in as `username`)