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
  * `redis_bind_addresses`: Interface on which Redis will listen (default: `0.0.0.0`)
