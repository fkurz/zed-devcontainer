# zed-remote-docker

## Building the image

```bash
docker build \
    --tag friedrichkurz.me/zeddevcon \
    --file .devcontainer/Dockerfile \
    --build-arg="THE_USER_NAME=zed" \
      --build-arg="THE_USER_PASSWORD=zed" \
      --build-arg="THE_USER_UID=$(id -u $(whoami))" \
      --build-arg="THE_SSH_PORT=2022" \
    .
```

## Create SSH config entry

```conf
Host zeddevcon
  HostName localhost
  Port 2022
  User zed
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
```

## Running container with devcontainer CLI

```bash
devcontainer --workspace-folder=. up
```

> (i) Test the SSH connection with e.g.
>
> ```bash
> ssh zeddevcon 'echo "$(whoami)"'
> Warning: Permanently added '[localhost]:2022' (ED25519) to the list of known hosts.
> zed@localhost's password:
> zed
> ```
>
> When prompted, enter the SSH user's password (`zed` in this case).
> The expected output is the name of the SSH user configured in the OpenSSH configuration (i.e. `zed`).

## Connecting to the running devcontainer with SSH

```bash
ssh zeddevcon
```

> (i) When prompted, enter the SSH user's password (`zed` in this case).

## Connecting to the devcontainer with Zed over SSH

```bash
zed ssh://zeddevcon/workspace
```

This will automatically open a Zed editor window, connect over SSH (on the first login, Zed will also install the editor backend in the container), and change directory to the `/workspace` folder (where we mounted the current working directory).
