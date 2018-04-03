# Ansible Role: Docker Installer

[![CircleCI](https://circleci.com/gh/s3than/ansible-docker/tree/master.svg?style=svg)](https://circleci.com/gh/s3than/ansible-docker/tree/master)

An Ansible role that installs Docker on Ubuntu

## Requirements

If using host connection for tcp it is recommended to use {{ docker_tls }}

## Role Variables

Available variables are listed below, along with default values:

    ---
    docker_package: 'docker-ce'
    docker_version: '17*'

    # Docker Compose options.
    docker_install_compose: true
    docker_compose_version: '1.14.0'

    # Daemon configuration
    docker_icc: "false"
    docker_live_restore: "true"
    docker_log_level: 'info'
    docker_disable_legacy_registry: "true"

    # Testing will use and deploy vfs
    # Default to overlay 2'
    docker_storage_driver: 'overlay2'

    docker_storage_opts: []

    # If setting to local only remove tcp connections
    # and set docker_generate_tls to False if running a tcp with
    # generating tls certs no security will be available on the endpoint
    docker_hosts:
      - 'unix:///var/run/docker.sock'
      # - 'tcp://0.0.0.0:2376'

    docker_graph: '/var/lib/docker'

    # This is set to default to attempt to enforce security
    # however if someone sets tcp hosts and sets this to true
    # the endpoint will have no security
    docker_tls: "false"
    docker_tlscacert: ""
    # docker_tlscacert: "ca.crt"
    docker_tlskey: ""
    # docker_tlskey: ".key"
    docker_tlscert: ""
    # docker_tlscert: ".crt"
    docker_tlsverify: "false"

    # Start User Namespace mapping
    # Username and Group name for userns, defult uses
    # dockremap as the username and groupname
    docker_userns_remap: "default"

    docker_dns_add_local: true

    docker_dns:
      - 8.8.8.8
      - 8.8.4.4

    # Variable to configure the which type of logging to use
    # Options: "json-file", "gelf"
    docker_log_driver: 'json-file'
    docker_log_options: {}
    # Example gelf driver
    # docker_log_options:
    #   - gelf-address: "udp://1.2.3.4:12201"
    #   - labels: 'production_status,geo'
    # Log options documentation can be found here:
    # https://docs.docker.com/engine/admin/logging/gelf/#usage
    # https://docs.docker.com/engine/admin/logging/json-file/#options

## Dependencies

If using for having a tcp end point available then tls then using a role to install easyrsa certificates is recommended

## Example Playbook

    ---
    - name: Configuration Playbook
      hosts: all
      roles:
        - docker
