# ansible-role-redis-cluster
**Under Testing nearly done to be used in production**

[![CI](https://github.com/netways/ansible-role-redis-cluster/workflows/Molecule%20Test/badge.svg?event=push)](https://github.com/netways/ansible-role-redis-cluster/workflows/Molecule%20Test/badge.svg)

This role will install one to  x redis instances on one to x hosts as either standalone or cluster.

SELinux will be managed if it is enabled.

The current version is tested on rocky 8.

You can use the [ca role](https://github.com/NETWAYS/ansible-role-ca), if you want to use self singed certificate.

## Requirements ##

* policycoreutils-python-utils if SELinux is enabled.

## Variables ##

* `manage_dnf_module`: Should the corresponding dnf module enabled? (default: `true`)
* `redis_version`: Redis version to be installed (default: `6`)
* `kernel_virtual_memory`: If you want to disable the kernel virtual memory, set the value to 1 (default: `undefined, the system default will be used`)
* `transparent_hugepages_value`: Manage THP, availible values are `always`, `madvise` or `never` (default: `undefined, the system default will be used`)
* `redis_systemd_directory`: Directory for redis systemd service (default: `/usr/lib/systemd/system`)
* `redis_config_directory`: Directory for redis configuration file (default: `/etc/redis`)
* `redis_user_name`: User as runner of redis service and owner of redis files\directories (default: `redis`)
* `redis_group_name`: Group as runner of redis service and owner of redis files\directories (default: `redis`)
* `redis_installation_scenario`: To build a cluster and to be able to set the `ALL` later cluster related configurations set it to `cluster` (default: `standalone`)
* `redis_masters_replicas`: Replica nummber to each redis master (default: `1`)
* `redis_requirepass`: Protect the connection to masters or instances with a password (default: `changeme`)
* `redis_masterauth`: If you use a cluster, set the same requirepass value here to let replicas authenticate to the masters (default: `changeme`)
* `redis_configurations`: A list of redis configurations to one or many redis instances. It includs the following parameters:
  * `redis_instance_name`: Redis instance name. It should be uniq. The service and the redis config file will be accordingly named. (default: `redis01`)
  * `redis_bind_addresses`: Interface on which redis will listen (default: `0.0.0.0`)
  * `redis_port`: Port on which redis will listen. Set it to 0 if you will use only TLS (default: `7010`)
  * `redis_appendonly`: If enabled, it affords better data durability at the cost of slightly slower performance (default: `no`)
  * `redis_appendfilename`: The name of the append only file (default: `appendonly_redis01.aof`)
  * `redis_protected_mode`: Disable it to let clients connect to your instance without authentication (default: `yes`)
  * `redis_pidfile`: Path to pidfile (default: `/var/run/redis/redis01.pid`)
  * `redis_logfile`: Path to logfile (default: `/var/log/redis/redis01.log`)
  * `redis_loglevel`: Log level (default: `notice`)
  * `redis_cluster_config_file`: Path to cluster file (default: `undefined`) (recommendation: `/etc/redis/redis01_cluster.conf`)
  * `redis_cluster_enabled`: Run in cluster mode? (default: `undefined`).
  * `redis_cluster_node_timeout`: Consider a node in failure state, if it is unreachable after this time in milliseconds (default: undefined)
  * `redis_daemonize`: Run redis as adaemon and write a pid file (default: `yes`)
  * `redis_databases`: Set the number of databases (default: `16`)
  * `redis_save`: Redis Will save the DB on disk if both the given number of seconds and the given number of write operations against the DB occurred. In the example below the behavior will be to save after 900 sec (15 min) if at least 1 key changed, etcetera. The default is:
    ```yaml redis_save:
    - '900 1'
    - '300 10'
    - '60 10000'
    ```
  * `redis_rdbcompression`: Compress string objects using LZF when dump .rdb databases? (default: `yes`)
  * `redis_dbfilename`: The filename where to dump the DB (default: `redis01_dump.rdb`)
  * `redis_dbdir`: The path of DB (default: `/var/lib/redis`)
  * `redis_tls_port`: TLS-listening port, `redis_port` should be set to `0` (default: undefined). All other possible TLS parameters mentioned in this documention requier this value to be set.
  * `redis_tls_cert_file`: Path to certificate file (default: `undefined`, recommendation: `/etc/redis/redis_certificates/{{ ansible_fqdn }}.crt`)
  * `redis_tls_key_file`: Path to key file (default: `undefined`, recommendation: `/etc/redis/redis_certificates/{{ ansible_fqdn }}.key`)
  * `redis_tls_ca_cert_file`: Path to CA file (default: `undefined`, recommendation: `/etc/redis/redis_certificates/ca.crt`)
  * `redis_tls_auth_clients`: If `no` is specified, client certificates are not required and not accepted. If `optional` is specified, client certificates are accepted and must be valid if provided, but are not required. (default: `undefined`)
  * `redis_tls_cluster`: If to enable TLS for the cluster bus protocol (default: `undefined`)
  * `redis_tls_replication`: Let replica to establish a TLS connection with its master (default: `undefined`)
  * `redis_includes`: Include one or more other standard config files (default: `undefined`)
  * `redis_loadmodule`: Load modules at startup. If the server is not able to load modules, it will abort. It is possible to use multiple loadmodule directives (default: `undefined`)
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
## License ##
BSD
