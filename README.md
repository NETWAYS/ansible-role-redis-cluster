# ansible-role-redis-cluster
**UnderTesting nearly done to be used in production**

This role will install one to  x redis instances on one to x hosts as either as standalone or as a cluster.

SELinux will be managed if it is enabled.

The current version is tested on rocky 8.

## Requirements ##

* policycoreutils-python-utils if SELinux is enabled.

## Variables ##

* `manage_dnf_module`: Should the corresponding dnf module enabled? (default: `true`)
* `redis_version`: Redis version to be installed (default: `6`)
* `kernel_virtual_memory`: If you want to disable the kernel virtual memory set the value to 1 (default: `unset, the system default will be used`)
* `transparent_hugepages_value`: Manage THP (default: `unset, the system default will be used`)
* `redis_systemd_directory`: Directory for redis systemd service (default: `/usr/lib/systemd/system`)
* `redis_config_directory`: Directory for redis configuration file (default: `/etc/redis`)
* `redis_user_name`: User as runner of redis service and owner of redis files\directories (default: `redis`)
* `redis_group_name`: Group as runner of redis service and owner of redis files\directories (default: `redis`)
* `redis_installation_scenario`: To build a cluster set it to empty string or foo to lest redis instances as standalone (default: `cluster`)
* `redis_masters_replicas`: Replica nummber to each redis master (default: `1`)
* `redis_requirepass`: Protect the connection to masters or instances with a password (default: `changeme`)
* `redis_masterauth`: If you use a cluster, set the same requirepass as avalue here to let the replica authenticate to the master (default: `changeme`)
* `redis_configurations`: A list of redis configurations to one or many redis instances. It includs the following parameters:
  * `redis_instance_name`: Redis instance name. It should be uniq. The service and the redis config file will have the accordingly named.
  * `redis_bind_addresses`: Interface on which redis will listen (default: `0.0.0.0`)
  * `redis_port`: Port an which redis will listen. Set it to 0 if you will use TLS (default: `0`)
  * `redis_appendonly`: Enable or disable beeter date durability (default: `no`)
  * `redis_protected_mode`: Disable it to let clients connect to your instance without authentication (default: `yes`)
  * `redis_pidfile`: Path to pidfile (default: `/var/run/redis/{{ redis_instance_name }}.pid`)
  * `redis_logfile`: Path to logfile (default: `/var/log/redis/{{ redis_instance_name }}.log`)
  * `redis_loglevel`: Log level (default: `notice`)
  * `redis_cluster_config_file`: Path to cluster file (default: `/etc/redis/{{ redis_instance_name }}_cluster.conf`)
  * `redis_cluster_enabled`: Run in cluster mode? (default: `yes`). It will be set only if `redis_installation_scenario` set to `cluster`
  * `redis_cluster_node_timeout`: Is an amount of milliseconds a node must be unreachable for it to be considered in failure state. After this time a replica will be elected as amaster (default: `5000`)
  * `redis_daemonize`: Run redis as adaemon and write a pid file (default: `yes`)
  * `redis_databases`: Set the number of databases (default: `16`)
  * `redis_save`: Redis Will save the DB if both the given number of seconds and the given number of write operations against the DB occurred. In the example below the behavior will be to save after 900 sec (15 min) if at least 1 key changed, etcetera. The default is:
    ```yaml redis_save:
	      - '900 1'
              - '300 10'
              - '60 10000'
    ```
  * `redis_rdbcompression`: Compress string objects using LZF when dump .rdb databases? (default: `yes`)
  * `redis_dbfilename`: The filename where to dump the DB (default: `{{ redis_instance_name }}_dump.rdb`)
  * `redis_dbdir`: The path of DB (default: `/var/lib/redis`)
  * `redis_tls_port`: TLS-listening port, `redis_port` should be set to `0`
  * `redis_tls_cert_file`: Path to certificate file (default: `/etc/redis/redis_certificates/{{ ansible_fqdn }}.crt`)
  * `redis_tls_key_file`: Path to key file (default: `/etc/redis/redis_certificates/{{ ansible_fqdn }}.key`)
  * `redis_tls_ca_cert_file`: Path to CA file (default: `/etc/redis/redis_certificates/ca.crt`)
  * `redis_tls_auth_clients`: If "no" is specified, client certificates are not required and not accepted. If "optional" is specified, client certificates are accepted and must be valid if provided, but are not required. (default: `no`)
  * `redis_tls_cluster`: If to enable TLS for the cluster bus protocol (default: `yes`)
  * `redis_tls_replication`: Let replica to establish a TLS connection with its master (default: `yes`)
  * `redis_extra_config`: Set your configuration lines to the end of redis configuration file, for example:
    ```yaml redis_extra_config:
      - repl-diskless-sync no
      - repl-diskless-sync-delay 5
      - repl-diskless-load disabled
    ```
## Example Playbook ##

```yaml
---
- hosts: redis
  become: yes
  roles:
    - redis_cluster
```
