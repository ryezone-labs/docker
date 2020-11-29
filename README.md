[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/ryezone-labs/docker)

ryezone_labs.docker
=========

This role installs docker and provisions a swarm cluster.

Role Variables
--------------

| Variable Name | Type | Default Value | Description |
| ------------- | ---- | ------------- | ----------- |
| `docker_compose_version` | string | `1.23.1` | Version of docker-compose to install. |
| `docker_compose_checksum` | string | `sha256:c176543737b8aea762022245f0f4d58781d3cb1b072bc14f3f8e5bb96f90f1a2` | SHA-256 hash of the docker-compose executable. |
| `docker_repository` | string | `deb [arch={{ docker_arch }}] {{ docker repository_url }} {{ docker_ubuntu_version }} {{ docker_release_type }}` | Format string that builds the docker apt repository to install. |
| `docker_package` | string | `{{ docker_product }}={{ docker_version }}` | Format string that builds the docker package to install. |
| `docker_repository_url` | string | `https://download.docker.com/linux/ubuntu` | URI for the docker apt repository. |
| `docker_repository_key_url` | string | `https://download.docker.com/linux/ubuntu/gpg` | URI for the docker apt repository key. |
| `docker_repository_key_id` | string | `9DC858229FC7DD38854AE2D88D81803C0EBFCD88` | Key Id for the docker apt repository key. |
| `docker_arch` | string | `amd64` | Architechture build of docker to install Used to select correct apt repository. |
| `docker_ubuntu_version` | string | `bionic` | Ubuntu release name.  Used to select the correct apt repository. |
| `docker_release_type` | string | `stable` | Selects which docker release branch to install.  Valid values include: `edge`, `nightly`, `stable`, `test` |
| `docker_product` | string | `docker-ce` | Selects the docker product to install.  Valid values include: `docker-ce` and `docker-ee` |
| `docker_version` | string | `5:18.09.4~3-0~ubuntu-bionic` | Docker package version to install.  Valid values can be listed by running `apt-cache madison docker-ce` |
| `docker_users` | Array of string | `[ 'vagrant' ]` | This is a list of users and groups to add to the `docker` group and grant access to the docker socket. |
| `docker_swarm_enabled` | bool | `false` | Enables docker swarm mode. |
| `docker_swarm_node_type` | string | `manager` | Configures node as either a `manager` or a `worker` when `{{ docker_swarm_enabled }}` |
| `docker_swarm_initial_manager` | string | `''` | Indicates the initial swarm manager node from which docker swarm join tokens will be requested. This value should be `''` on the initial manager. |
| `docker_swarm_initial_manager_ssh_username` | string | `''` | Username to use to request swarm join tokens over ssh. |
| `docker_swarm_initial_manager_ssh_private_key` | string | `''` | Private SSH Key to install for `{{ docker_swarm_initial_manager_ssh_username }}` that allows other docker nodes to request join tokens. |
| `docker_swarm_initial_manager_ssh_public_key` | string | `''` | Public SSH key to install in the authorized_keys for `{{ docker_swarm_initial_manager_ssh_username }}` to allow other docker nodes to request join tokens. |

Example Playbook
----------------

## Standalone Docker Host

```yaml
---
- hosts: 127.0.0.1
  connection: local
  vars_files:
    - "/opt/ansible/vars/common.yml"
  vars:
    desktop: false
  roles:
    - rz.packer_init
    - rz.docker
    - rz.packer_complete
```

License
-------

BSD
