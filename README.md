# zed-remote-docker

## Building the image

```bash
docker build -t friedrichkurz.me/zed-devcontainer -f .devcontainer/Dockerfile --build-arg="THE_CONTAINER_USER_ID=$(id -u $(whoami))" .
```

## Running container with devcontainer CLI

```bash
devcontainer up --workspace-folder=.
```

## Connecting to the running devcontainer with SSH

```bash
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -l "thecontaineruser" -p 65432 localhost
```

Enter "thecontaineruserpassword" when prompted.

## Connecting to the devcontainer with Zed

```bash
ssh-keygen -R '[localhost]:65432'
zed ssh://thecontaineruser:thecontaineruserpassword@localhost:65432/workspace
```
