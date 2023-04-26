sleif.nextcloud_container
============

This role runs a nextcloud instance on docker.

Requirements
------------

Use it on a machine setup with ansible role sleif.docker, sleif.redis_docker and sleif.mariadb_docker.

Role Variables
--------------

See defaults/main.yml for customination.

Dependencies
------------

- geerlingguy.docker to make sure Docker is available.
- sleif.docker to make sure Docker is configured as needed.
- sleif.redis_docker
- sleif.mariadb_docker

Example Playbook
----------------

    - hosts: "server"
      user: root
      vars:
        DOCKER_NETWORK_NAME: 'custom_docker_network'
      roles:
        - { role: sleif.redis_docker, tags: "redis_docker" }
        - { role: sleif.mariadb_docker, tags: "mariadb_docker" }
        - { role: sleif.nextcloud_container, tags: "nextcloud_container",
                                                      NEXTCLOUD_VERSION: "21",
                                                      MYSQL_USER: "mysql_db_user",
                                                      MYSQL_PASSWORD: "mysql_db_user_password",
                                                      MYSQL_DATABASE: "mariadb_nextcloud_database",
                                                      VIRTUAL_HOST: "cloud.example.com",
                                                      LETSENCRYPT_EMAIL: "letsencrypt@example.com" }

License
-------

MIT

Author Information
------------------

Created in 2021 by Sebastian Berthold
