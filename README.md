# ansible-jitsi

Installs jitsi via several docker container.

This role bases on [docker-jitsi-meet@github](https://github.com/jitsi/docker-jitsi-meet)

Tested on:
* Ubuntu 16.04

## System requirements

* Docker
* docker-compose
* Systemd

## Role requirements

* python-docker package

## Tasks

* Build docker image locally
* Create volume paths for docker container
* Create docker-compose file
* Setup systemd unit file
* Start/Restart service

## Role parameters

| Variable      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| volume        | text | no         | <empty> | Local path to jitsi config and data |
| publish.web   | text | no         | <empty> | Web Port to be published            |
| publish.https | text | no         | <empty> | Https Port to be published (but not used) |
| publish.interface | text | no     | 0.0.0.0 | Interface to be published                 |
| publish.url       | text | yes    | <empty> | Public url                                |
| users             | array of User | yes | [] | User configuration                       |
| force             | boolean       | no  | no | Force to re-create volumes and configuration |

### User type definition

| Property      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| username      | text | yes        | <empty> | The user's username   |
| password      | text | yes        | <empty> | The user's password (in clear text!) |

Please notice: prosody is saving your passwords in clear text! Be aware using generated passwords or similar.

## Usage

### requirements.yml

```yaml
- name: install-jitsi
  src: https://github.com/borisskert/ansible-meet-jitsi.git
  scm: git
```

### Example Playbook

Usage

```yaml
    - role: install-jitsi
      volume: /srv/docker/jitsi
      force: no
      publish:
        web: 10080
        https: 10443
        interface: "{{docker_network_interface}}"
        url: 'https://meet.my.jitsi'
      users:
        - username: myuser
          password: mypassword
        - username: myuser2
          password: mypassword2
```

# Test this role

Requirements:

* ansible
* vagrant
* VirtualBox

```shell script
$ cd tests
$ ./test.sh
```
