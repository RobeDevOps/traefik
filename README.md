robedevops.traefik
=========

Install Traefik proxy and default version is 1.7.10

Requirements
------------

If any provider is going to be used need to be installed: e.g docker.

Role Variables
--------------
It is possible to specify a traefik version with linux flavor and architecture. This allows a more flexible solution in order to install a version from this traefik release list [here](https://github.com/containous/traefik/releases)

```
traefik_version: 1.7.10
traefik_flavor: linux
traefik_arch: amd64
traefik_binary_url: https://github.com/containous/traefik/releases/download/v{{ traefik_version }}/traefik_{{ traefik_flavor }}-{{ traefik_arch }}
```

In order to configure the binary and configuration directory using the variables:

```
traefik_install_directory: /usr/bin
traefik_bin: "{{ traefik_install_directory }}/traefik"
traefik_config_directory: /etc/traefik
traefik_config_file: traefik.toml
traefik_config_file_path: "{{ traefik_config_directory }}/{{ traefik_config_file }}"
```

This role creates a systemd service and the name and path can be modidied using the **traefik_systemd_service_** variables

```
traefik_systemd_service_name: traefik.service
traefik_systemd_service_home: /etc/systemd/system/
traefik_systemd_service_path: "{{ traefik_systemd_service_home }}/{{ traefik_systemd_service_name }}"
```

By default **traefik_upgrade_version** is no but it has to be changed when new version is installed. This variable is used to stop the **traefik service** that is running current version and it is a safety steps in the upgrade process.

```
traefik_upgrade_version: no
```

Below is the logs configuration. The json is an option and it is not required but I prefer json format for troubleshooting logs as objects. :D

```
traefik_log_level: INFO
traefik_log_path: /var/log/traefik.log
traefik_log_format: json
```

Working on the api configuration with more details, but so far is possible to enable the api using this:

```
traefik_api_enable: true
```

If docker provider is used then docker should be installed on host. Can use any docker role on ansible-galaxy repository before running this role. 

```
traefik_docker_provider: true
traefik_docker_endpoint: unix:///var/run/docker.sock
traefik_docker_domain: localhost
traefik_docker_swarm_mode: true
traefik_docker_network: traefik
```

In order to check the traefik availability the **ping configuration** is used, if value is not provided default will be **ping** and if port for healthcheck is not provided default will be 8082. All these can be modified with:

```
traefik_healthcheck_entrypoint: ping
traefik_healthcheck_port: 8082
```

Dependencies
------------

See requirements. Only if providers are going to be used.

Running a playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
  - hosts: traefik
    roles:
        - robedevops.traefik
```

License
-------

BSD

Author Information
------------------

Roberto Cardenas Isla - email: rcardenas20@gmail.com
