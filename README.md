# zed-remote-docker

> (!) This is just a simple proof of concept for [Zed remote development][1] using [dev containers][2] and not intended (nor prepared) for any real world development scenarios.

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

> (i) The image architecture largely follows [my previous project][3].

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

> (i) In case the container start was successful, you will see something like
>
> ```bash
> [1 ms] @devcontainers/cli 0.72.0. Node.js v23.5.0. darwin 23.4.0 arm64.
> [261 ms] Start: Run: docker run --sig-proxy=false -a STDOUT -a STDERR --mount type=bind,source=/private/tmp/zed-devcontainer,target=/workspaces/zed-devcontainer,consistency=cached --mount type=bind,src=/private/tmp/zed-devcontainer,dst=/workspace -l devcontainer.local_folder=/private/tmp/zed-devcontainer -l devcontainer.config_file=/private/tmp/zed-devcontainer/.devcontainer/devcontainer.json --network=host --name=zeddevcon --entrypoint /bin/sh -l devcontainer.metadata=[{"mounts":[{"source":"${localWorkspaceFolder}","target":"/workspace","type":"bind"}],"overrideCommand":false,"forwardPorts":[2022]}] zeddevcon -c echo Container started
> Container started
> {"outcome":"success","containerId":"1fc60a61b2f6f2408a6ad97a3e19982881e1cf2959daeb166753aa8ae02a0c9b","remoteUser":"root","remoteWorkspaceFolder":"/workspaces/zed-devcontainer"}
> ```
>
> (i) Test that you can connect to the running container with SSH e.g.
>
> ```bash
> ssh zeddevcon 'echo "$(whoami)"'
> ```
>
> When prompted, enter the SSH user's password (`zed` in this example).
> The name of the SSH user (i.e. `zed` in this example) should be printed to output.
>
> (i) More on the Devcontainer spec and Devcontainer CLI [on the offical website][2].

## Attaching a shell to the running devcontainer

```bash
ssh zeddevcon
```

> (i) When prompted, enter the SSH user's password (i.e. `zed` in this example).

If you have [`sshpass`][4] installed, you may skip the password input step and directly connect to the container as follows:

```bash
sshpass -p zed zeddevcon
```

## Connecting to the devcontainer with Zed over SSH

```bash
zed ssh://zeddevcon/workspace
```

This will automatically open a Zed editor window, connect over SSH (on the first login, Zed will also install the editor backend in the container), and change directory to the `/workspace` folder (where we mounted the current working directory).

Zed also provides the very convenient option to directly pass the user and password in the connection URL. So, if you're looking to open a Zed editor window and directly authenticate, invoke the Zed CLI as follows:

```bash
zed ssh://zed:zed@zeddevcon/workspace
```

> (i) More on Zed remote development [in the official documentation][1].

[1]: https://zed.dev/docs/remote-development
[2]: https://containers.dev/
[3]: https://github.com/fkurz/devcontainer-ssh
[4]: https://linux.die.net/man/1/sshpass
