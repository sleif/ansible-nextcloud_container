# sleif.nextcloud_container

This role runs a Nextcloud instance on Podman.

## Requirements

Use it on a machine setup with ansible roles sleif.podman, sleif.caddy_container, sleif.redis_container and sleif.mariadb_container.

## Role Variables

See defaults/main.yml for customization.

### SE Linux related

- If necessary make use of `container_volumes_parameter` which can contain values like `:Z`. It defaults to `''`.

## Dependencies

```sh
ansible-galaxy install sleif.podman --force
ansible-galaxy install sleif.caddy_container --force
ansible-galaxy install sleif.redis_container --force
ansible-galaxy install sleif.mariadb_container --force
ansible-galaxy install sleif.nextcloud_container --force
```

## Example Playbook

```yml
- name: VM cloud.example.com
  hosts: "cloud.example.com"
  user: root
  vars:
    podman_networks:
      podman_network_root:
        podman_network_name: 'podman_custom'
        podman_network_subnet: '10.1.0.0/16'
        podman_network_gateway: '10.1.0.1'
      podman_network_rootless:
        podman_network_name: 'podman_custom_rootless'
        podman_network_subnet: '10.2.0.0/16'
        podman_network_gateway: '10.2.0.1'
    podman_rootless: true
    podman_network_name: "{{ podman_networks.podman_network_rootless.podman_network_name if podman_rootless | bool else
      podman_networks.podman_network_root.podman_network_name }}"

  roles:
    - {role: sleif.podman, tags: "podman_role",
       podman_operation: "podman_install"}
    - {role: sleif.redis_container, tags: "redis_container",
       pod_name: "nextcloud-pod"}
    - {role: sleif.mariadb_container, tags: "mariadb_container",
       pod_name: "nextcloud-pod"}
    - {role: sleif.nextcloud_container, tags: "nextcloud_container",
       pod_name: "nextcloud-pod"}
```

## License

MIT

## Author Information

Created in 2023 by Sebastian Berthold
