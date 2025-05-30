FROM ubuntu:24.04

LABEL docker.io/image=friedrichkurz.me/zed-devcontainer

ARG THE_CONTAINER_USER_NAME="thecontaineruser"
ARG THE_CONTAINER_USER_PASSWORD="thecontaineruserpassword"
ARG THE_CONTAINER_USER_ID="1000"
ARG THE_SSH_PORT="65432"

COPY "./.devcontainer/sshd_config.template" "/tmp/sshd_config.template"

SHELL ["/usr/bin/bash", "-c"]

RUN <<EOF
set -euo pipefail
apt-get update
# Add SSH user with password for SSH authentication with root privileges
useradd -m -d "/home/${THE_CONTAINER_USER_NAME}" -s /bin/bash -g sudo -G sudo -u "${THE_CONTAINER_USER_ID}" "${THE_CONTAINER_USER_NAME}"
printf "${THE_CONTAINER_USER_NAME}:${THE_CONTAINER_USER_PASSWORD}" | chpasswd
# Install OpenSSH server
# See https://github.com/fkurz/devcontainer-ssh/blob/main/.devcontainer/base/Dockerfile
apt-get update
apt-get install --yes gettext-base openssh-server sudo
mkdir -p /etc/ssh
mkdir -p /var/run/sshd
mkdir -p /etc/ssh/sshd_config.d
cat /tmp/sshd_config.template \
  | envsubst '$THE_SSH_PORT $THE_CONTAINER_USER_NAME $THE_CONTAINER_USER_PASSWORD' \
  | tee /etc/ssh/sshd_config.d/devcontainer.conf
apt-get remove --yes gettext-base
rm /tmp/sshd_config.template
command -v sshd
EOF

ENTRYPOINT ["/usr/sbin/sshd", "-D"]

EXPOSE ${THE_SSH_PORT}
