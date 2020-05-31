# ansible-jitsi

Installs jitsi via several docker container.

This role bases on [docker-jitsi-meet@github](https://github.com/jitsi/docker-jitsi-meet)

Supported operating systems:

* Ubuntu 16.04
* Ubuntu 18.04
* Ubuntu 20.04
* Debian 9
* Debian 10

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
| jitsi_volume        | text | no         | <empty> | Local path to jitsi config and data |
| jitsi_web_port   | text | no         | <empty> | Web Port to be published            |
| jitsi_https_port | text | no         | <empty> | Https Port to be published (but not used) |
| jitsi_web_interface | text | no     | 0.0.0.0 | Web interface to be published                 |
| jitsi_videobridge_interface | text | no     | 0.0.0.0 | Videobridge interface to be published     |
| jitsi_external_url                   | text | yes    | <empty> | Public url                                |
| jitsi_users                         | array of User | yes | [] | User configuration                       |
| jitsi_enable_auth                   | boolean | no  | no       | Enables authentication (enabled by default)  |
| jitsi_allow_guests                  | boolean | no  | no       | Enables guests (disabled by default)         |
| jitsi_force_recreate                         | boolean       | no  | no | Force to re-create volumes and configuration |
| jitsi_images_version                | text          | no  | latest | Specifies the docker images version      |
| jitsi_force_pull                    | boolean       | no  | no     | Forces the re-pull of the docker images  |

### User type definition

| Property      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| username      | text | yes        | <empty> | The user's username   |
| password      | text | yes        | <empty> | The user's password (in clear text!) |

Please notice: prosody is saving your passwords in clear text! Be aware using generated passwords or similar.

## Usage

### Add to `requirements.yml`

```yaml
- name: install-jitsi
  src: https://github.com/borisskert/ansible-meet-jitsi.git
  scm: git
```

### Minimal `playbook.yml`

```yaml
- hosts: test_machine
  become: yes

  roles:
    - role: install-jitsi
      jitsi_volume: /srv/docker/jitsi
      jitsi_web_port: 10080
      jitsi_https_port: 10443
      jitsi_external_url: http://192.168.33.10:10080
```

### Typical `playbook.yml`

```yaml
    - role: install-jitsi
      jitsi_volume: /srv/docker/jitsi
      jitsi_images_version: stable-4548-1
      jitsi_web_port: 10080
      jitsi_https_port: 10443
      jitsi_web_interface: 0.0.0.0
      jitsi_videobridge_interface: 0.0.0.0
      jitsi_external_url: http://localhost:10080
      jitsi_enable_auth: true
      jitsi_allow_guests: true
      jitsi_users:
        - username: myuser
          password: mypassword
        - username: myuser2
          password: mypassword2
      jitsi_force_recreate: false
```

## Testing

Requirements:

* [Vagrant](https://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [Ansible](https://docs.ansible.com/)
* [Molecule](https://molecule.readthedocs.io/en/latest/index.html)
* [yamllint](https://yamllint.readthedocs.io/en/stable/#)
* [ansible-lint](https://docs.ansible.com/ansible-lint/)
* [Docker](https://docs.docker.com/)

### Run within docker

```shell script
molecule test
```

### Run within Vagrant

```shell script
 molecule test --scenario-name vagrant --parallel
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

## Links

* [docker-jitsi-meet@github](https://github.com/jitsi/docker-jitsi-meet)
* [Official Jitsi meet](https://meet.jit.si)
