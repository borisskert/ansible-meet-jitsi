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
| volume        | text | no         | <empty> | Local path to jitsi config and data |
| publish.web   | text | no         | <empty> | Web Port to be published            |
| publish.https | text | no         | <empty> | Https Port to be published (but not used) |
| publish.web_interface | text | no     | 0.0.0.0 | Web interface to be published                 |
| publish.videobridge_interface | text | no     | 0.0.0.0 | Videobridge interface to be published     |
| publish.url                   | text | yes    | <empty> | Public url                                |
| users                         | array of User | yes | [] | User configuration                       |
| enable_auth                   | boolean | no  | no       | Enables authentication (enabled by default)  |
| allow_guests                  | boolean | no  | no       | Enables guests (disabled by default)         |
| force                         | boolean       | no  | no | Force to re-create volumes and configuration |
| images_version                | text          | no  | latest | Specifies the docker images version      |
| force_pull                    | boolean       | no  | no     | Forces the re-pull of the docker images  |

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
      volume: /srv/docker/jitsi
      publish:
        web: 10080
        https: 10443
        web_interface: 0.0.0.0
        videobridge_interface: 0.0.0.0
        url: http://192.168.33.10:10080
```

### Typical `playbook.yml`

```yaml
    - role: install-jitsi
      volume: /srv/docker/jitsi
      images_version: stable-4548-1
      publish:
        web: 10080
        https: 10443
        web_interface: "{{docker_network_interface}}"
        videobridge_interface: 0.0.0.0
        url: https://meet.my.jitsi
      enable_auth: yes
      allow_guests: no
      users:
        - username: myuser
          password: mypassword
        - username: myuser2
          password: mypassword2
      force: no
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
