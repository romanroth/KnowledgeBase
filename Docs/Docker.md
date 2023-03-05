# Docker

- **Docker Desktop Download:** [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
- **Docker Official Dokumentation:** [https://docs.docker.com](https://docs.docker.com)

## Docker Registry

> "The Registry is a stateless, highly scalable server side application that stores and lets you distribute Docker images."

See: [https://docs.docker.com/registry/](https://docs.docker.com/registry/)

**TODO: How to set up docker registry?**

When pulling a docker image on Windows the following error can occur: `Error response from daemon: Get "https://<address>:<port>/v2/": x509: certificate signed by unknown authority`. To generate a certificate use `keytool -printcert -sslserver nexus.sie.local:18079 -rfc`.

To fix this the certificate of the server hosting the registry must be added to the "Trusted Root Certificates" in the `certmgr.msc`. In there right click on "Trusted Root Certificates" -> "All Tasks" -> "Import" and the choose the certificate file and then add it. Then restart Docker (best done in Task Manager).

To build a Docker container in Jenkins refer to  [Build docker container and push to own registry](./Jenkins.md#build-docker-container-and-push-to-own-registry).

## Other

`docker cp` will keep the original file owner. Better use SCP:

- [https://www.linux.com/training-tutorials/ssh-scp-without-password-remote-host-look-ma-no-password/](https://www.linux.com/training-tutorials/ssh-scp-without-password-remote-host-look-ma-no-password/)
- [https://alvinalexander.com/linux-unix/how-use-scp-without-password-backups-copy/](https://alvinalexander.com/linux-unix/how-use-scp-without-password-backups-copy/)

- Docker self-hosted registry: [https://docs.docker.com/registry/deploying/](https://docs.docker.com/registry/deploying/)
- Docker certificates: [https://docs.docker.com/engine/security/certificates/](https://docs.docker.com/engine/security/certificates/)

Docker x509 error fix on windows:
- Import certificate into windows: [https://support.securly.com/hc/en-us/articles/360026808753-How-do-I-manually-install-the-Securly-SSL-certificate-on-Windows](https://support.securly.com/hc/en-us/articles/360026808753-How-do-I-manually-install-the-Securly-SSL-certificate-on-Windows)

Docker consumes a lot of RAM (vmmem.exe): [https://www.koskila.net/how-to-solve-vmmem-consuming-ungodly-amounts-of-ram-when-running-docker-on-wsl/](https://www.koskila.net/how-to-solve-vmmem-consuming-ungodly-amounts-of-ram-when-running-docker-on-wsl/)

Ports are not available (Docker Desktop Windows): [https://www.herlitz.io/2020/12/01/docker-error-ports-are-not-available-on-windows-10/](https://www.herlitz.io/2020/12/01/docker-error-ports-are-not-available-on-windows-10/)

```cmd
net stop winnat
net start winnat
```

- copy failed file not found in build context: [https://stackoverflow.com/a/69852718](https://stackoverflow.com/a/69852718)
  - docker context is the directory the Dockerfile is located in.
  - docker context is the argument at the end (here `.`): `docker build -f dockerfile -t container:v1 .`
